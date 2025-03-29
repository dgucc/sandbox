# Firefox

## deactivate CORS

> security.fileuri.strict_origin_policy = false  
> privacy.file_unique_origin = false  

for chrome :
`chrome.exe --user-data-dir="C:/folder" --disable-web-security `  

## language

intl.locale.requested = fr

## Markdown (md files)

- Install Extension Markdown Viewer Webkit

- Define mime-type in firefox

[Url] about:config

[Locate config file]

> helpers.private_mime_types_file: ~/.mime.types

[add text] 

> type=text/plain exts=md,mkd,mkdn,mdwn,mdown,markdown desc="Markdown document"

## homepage

As admin, C:\Program Files (x86)\Mozilla Firefox\mozilla.cfg :  
```
// Set Startpage
pref(browser.startup.homepage,file:///D:/tamere.html);
```
## Disable WEBP

about:config :  
`image.webp.enabled = false`  

