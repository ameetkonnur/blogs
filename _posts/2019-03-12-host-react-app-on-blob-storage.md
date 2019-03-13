---
title: host a react app out of azure blob storage
---

## The What

Wouldnt it be cool if you could host your entire Web Application front end without any Web Server!!, [Github App](https://yogimonkey.z29.web.core.windows.net/). This App runs directly on Azure Blob Storage without any Web Server. You would say its just static page. No, its Web Application that takes an input calls an Github API to get user information and displays it on the page. Now that it's possible, why do it ?

## The Why

- It saves a ton of money not hosting & managing a Web Server(s)
- It scales effeciently because it's serverless
- It integrates well with CDN to offload user traffic
- It can also handle state information to make your backend API's truly stateless
- And Yeah its cool!


## The How

Its relatively easy to do this. All you need is some basic understanding of React.js (i hear Vue.js also works on a similar concept), and a know how about how HTML, CSS, Async API calls work. Rest all of the work is done by React.js & Azure Blob Storage. Obviously you can build more complex applications extending this basic know how of how to run web pages (that use JS, HTML & CSS) from Blob Storage. There are quite a few customer who have done this on AWS using S3 buckets for their React apps. Thats the inspiration for me. So lets get started.


