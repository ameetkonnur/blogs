---
title: host a react app out of azure blob storage
---

## The What

Wouldnt it be cool if you could host your entire Web Application front end without any Web Server!!, [Github App](https://yogimonkey.z29.web.core.windows.net/). This App runs directly on Azure Blob Storage without any Web Server. You would say its just static page. No, its Web Application that calls an Github API to get user information and displays it on the page. Yes it's possible but why to do it ?

## The Why

- It saves a ton of money hosting & managing a Web Server
- It scales effeciently because it's serverless
- It integrates well with CDN to offload user traffic
- It can also handle state information to make your backend API's truly stateless
- Yeah and its cool!


## The How

Its relatively easy to do this. All you need is some basic JS (Javascript) expertise and a know how about how HTML, CSS, Async API calls work. Rest all of the work is done by Azure Blob Storage.
