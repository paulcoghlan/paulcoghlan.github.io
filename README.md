# paulcoghlan.github.io
Personal Blogging Site 

## Developing
Run local instance:

```
% hugo server -D
Start building sites … 
hugo v0.85.0+extended darwin/arm64 BuildDate=unknown
WARNING: calling IsSet with unsupported type "ptr" (*hugolib.SiteInfo) will always return false.

                   | EN  
-------------------+-----
  Pages            |  9  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     | 59  
  Processed images |  0  
  Aliases          |  1  
  Sitemaps         |  1  
  Cleaned          |  0  

Built in 53 ms
Watching for changes in /Users/paul/projects/paulcoghlan.github.io/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /Users/paul/projects/paulcoghlan.github.io/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

## Changing theme using mods

Install module

```sh
hugo mod init github.com/<your_user>/<your_project>
```

Update `config.toml`:

```yml
[module]
[[module.imports]]
  path = 'github.com/spf13/hyde'

```

Update modules:

```sh
hugo mod get -u
```