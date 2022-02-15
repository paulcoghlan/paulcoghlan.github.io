---
title: "Tips for using AWS Lambda Runtime Interface Emulator"
date: 2022-02-15T18:29:26Z
draft: false
tags:
    - aws
    - docker
    - tips
    - cloud
    - lambdas
---

## Scenario

I've found the [AWS Lambda Runtime Interface Emulator](https://github.com/aws/aws-lambda-runtime-interface-emulator) (RIE) to be a very useful tool to help develop custom Docker
images for AWS Lambdas - but I've not seen it mentioned much in blogs or StackOverflow, etc.

My scenario was that I needed to develop a service to read a SQS message containing a HTML link,
convert the HTML to PDF, and upload the PDF to S3.

I was using `wkhtmltopdf` HTML to PDF convertor (basically a headless Chrome Browser), with some SQS/S3
integration glue code in Kotlin to align with the rest of the codebase.

{{< figure src="/2022/convertor.png" width="320" >}}

## Why use a Lambda with a custom Docker image?

The easiest option to creating an AWS Lambda is to upload a ZIP file archive containing a raw Python,
Java, etc app.  However:

- You might want to use a runtime that AWS doesn’t currently support
- Or you’re running 3rd-party binaries (e.g `wkhtmltopdf` HTML-> PDF convertor, which
is fussy about Qt libraries)

## How to use a custom Docker image

Your AWS Lambda can trigger an executable in a Docker container.  Either:

- Base on a AWS-provided image with Runtime Interface Client (RIC) already included - see https://docs.aws.amazon.com/lambda/latest/dg/runtimes-images.html#runtimes-images-lp
- Or create your own custom image, but add a [Runtime Image Client](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-images.html#runtimes-api-client) (RIC) to interface Lambda services to your function code
e.g. for a Java Gradle build, add the following dependency:  
`implementation("com.amazonaws:aws-lambda-java-runtime-interface-client:1.0.0")`

## Why develop locally?

The main reason to develop images locally is that it offers a much faster development cycle - there
is no need to upload images to S3!

Logging issues following on from an incorrect Docker config are often easier to debug locally because
you can change the `Dockerfile` and see the effects almost immediately.  You can also inspect
the container itself as it is running on your local host.

## How to use the AWS Runtime Interface Emulator with your custom image

To recap: if you are using a custom Docker image you need a [Runtime Image Client](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-images.html#runtimes-api-client) (RIC) included in the deployed application.

Additionally if you are developing locally you need a [Runtime Interface Emulator](https://github.com/aws/aws-lambda-runtime-interface-emulator/) to expose a fake Lambda HTTP endpoint.  This RIE can be used
to trigger your function and verify that it will work as desired when it is deployed to AWS services.

I always find a diagram helps in these situations.  The figure below compares the architecture of a
real AWS Lambda function running in the cloud against an emulated local one running on your PC or laptop:

{{< figure src="/2022/localdev.png" >}}

The steps are as follows:

- Mount AWS Runtime Interface Emulator (RIE) directory and specify com.amazonaws.services.lambda.runtime.api.client.AWSLambda entrypoint:

```sh
# Install RIE - provides fake Lambda HTTP endpoint to trigger your function
mkdir -p ~/.aws-lambda-rie && \
    curl -Lo ~/.aws-lambda-rie/aws-lambda-rie https://github.com/aws/aws-lambda-runtime-interface-emulator/releases/latest/download/aws-lambda-rie && \
    chmod +x ~/.aws-lambda-rie/aws-lambda-rie   
```

- Run Docker image containing target function (`lambdas:latest` in this instance):

```sh
docker run -v ~/.aws-lambda-rie:/aws-lambda \ 
--entrypoint /aws-lambda/aws-lambda-rie -p 9000:8080 lambdas:latest /usr/bin/java \
-cp '.*:/opt/java/generate_pdf.jar' \
com.amazonaws.services.lambda.runtime.api.client.AWSLambda  lambdas.generatepdf.GeneratePDF::handleRequest
```

- Finally, you can invoke your Lambda function via the RIE endpoint:

```sh
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'
```

## Conclusion

- Lambdas are great - here we’ve isolated a 3rd party binary from the rest of our application infrastructure 🚀
- The AWS RIE framework is useful if you have Lambda config issues

## Some tips

- Watch out for Dockerfile ENTRYPOINT vs CMD! 🦶🔫 - see http://aws.amazon.com/blogs/opensource/demystifying-entrypoint-cmd-docker/
- You can quickly check that how your environment variables are setup from:

```sh
docker inspect --format='{{range .Config.Env}}{{println .}}{{end}}' <image> 
```

