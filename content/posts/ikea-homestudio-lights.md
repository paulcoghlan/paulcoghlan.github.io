---
title: "Ikea Homestudio Lights"
date: 2021-09-09T16:34:28+01:00
draft: false
tags:
    - hacks
    - ikea
    - audiovideo
    - zoom
---
I've built an automated home studio lighting setup for less than $100 which I believe produces more flattering images whilst being unobtrusive enough to look 
part of a typical desk environment.

My setup is based on two $39/£29 [IKEA Skurup](https://www.ikea.com/gb/en/p/skurup-work-wall-lamp-black-80471143/) desk lamps.

I bought some Osram Zigbee E14 LED lights from [Amazon UK](https://www.amazon.co.uk/gp/product/B0747VDHG3) when they were discounted to £4.49, and
with the help of a vice I very carefully popped the plastic cover off:

![Osram lamp with top cover off](/2021/IMG_4514.jpg)

(notice there are two bits of glue on the cover - maybe a heat gun would have made it easier?)

Here's what the modified bulb looks like in the lamp:
![Bulb in situ](/2021/no-cover.jpg)

I ordered a 3mm thick acrylic disc cut to the measured size of the lamp interior (117mm) from [SimplyPlastics](https://www.simplyplastics.com/catalog/discs/cast-acrylic-round-discs/led-light-diffusing-opal-acrylic-disc/c-24/c-93/p-962).  I think the result isn't too shabby:

![Ikea lamp with diffuser](/2021/finished-product.jpg)

Here are some comparison pics:

{{< figure src="/2021/original-lamp.jpg" title="Sample taken with original, unmodified bulb - note nasty shadows from a narrow light source.">}}

{{< figure src="/2021/with-disc.jpg" title="Sample taken with modified bulb and diffuser disc - note nice soft shadows.">}}

{{< figure src="/2021/20w-led.jpg" title="Sample taken with 20W LED from a video lighting kit - the output is brighter, but I think our modified lamp output looks almost as good.">}}  

The great thing about using Zigbee lights is that I can automate the whole setup and control all of the lights and the camera from a button (I'm using [Home Assistant](https://www.home-assistant.io/).  The lights can be automatically configured to the correct color temperature for the time of day.  More to come on that in another post!

