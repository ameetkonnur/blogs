---
title: host a react app out of azure blob storage
---

## The What

Wouldnt it be cool if you could host your entire Web Application front end without any Web Server!!, [Github App](https://yogimonkey.z29.web.core.windows.net/). This App runs directly on Azure Blob Storage without any Web Server. You would say its just static page. No, its a Web Application that takes an input calls a Github API to get user information and displays it on the page. Thats okay but, why do it ?

## The Why

- It saves a ton of money not hosting & managing Web Server(s)
- It scales effeciently because it's serverless
- It integrates well with CDN to offload user traffic
- It can also handle state information to make your backend API's truly stateless
- And Yeah its cool!


## The How

Its relatively easy to do this. All you need is some basic understanding of React.js (i hear Vue.js also works on a similar concept), and a know how about how HTML, CSS, Async API calls work. Rest all of the work is done by React.js & Azure Blob Storage. Obviously you can build more complex applications extending this basic know how of how to run web pages (that use JS, HTML & CSS) from Blob Storage. There are quite a few customer who have done this on AWS using S3 buckets for their React apps. Thats the inspiration for me. 

### Pre-requisites

What you need on your machine is
- node.js (you can install it from <https://nodejs.org>)
- Some editor like VS Code, Sublime etc (get VS Code from <https://code.visualstudio.com/>)
- Azure Subscription with rights to create Storage Account (get a free trail from <https://azure.microsoft.com>)

### Set it up

- Step 1

Install react.js on your machine. You can do this by opening command line (or bash) & running "npm install create-react-app"
I would strongly recommend you go through <https://reactjs.org/tutorial/tutorial.html> to understand React.js architecture and basics. This post will not cover too much about React concepts but there are many resources on the Internet to understand React. Pluralsight has got a few good courses too. These resources are summarized at the end of the post.

- Step 2

On the same command line run "create-react-app <myappname>". This will create a folder with the name of the Application and will install dependencies for creating a React based application. You can explore the folder structure to understand how these components interact with each other.

![create react app](https://github.com/ameetkonnur/blogs/raw/master/img/react-1.gif)

![folder structure](https://github.com/ameetkonnur/blogs/raw/master/img/react-2.gif)

- Step 3

On the command line run "npm start". This will run the React app and open up the browser and take you to the default page.

![npm start](https://github.com/ameetkonnur/blogs/raw/master/img/react-3.gif)

![App in Browser](https://github.com/ameetkonnur/blogs/raw/master/img/react-4.gif)

- Step 4

Before we deploy the App to storage, we will need to get a deployable code which works directly off storage. For that on the command line run "npm run build". This creates a deployable code under the folder "build" in the same directory.

![npm run build](https://github.com/ameetkonnur/blogs/raw/master/img/react-5.gif)

![build folder](https://github.com/ameetkonnur/blogs/raw/master/img/react-6.gif)

You can now copy the contents entire build folder to any Web Server and the application would run as is. But wait!, the entire point of this blog is not to use the Web Server. That's where Azure Blob Storage comes in. Let's now move to the next steps where we create Azure Blob Storage, copy the files & create static website out of it.

- Step 5

Go to <https://portal.azure.com> and login. Click on the Create Resource + sign. Create a storage account. (<https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal>)

- Step 6

Go to the storage account and navigate on the menu blade to the "static website" option.
Enable the static site option and put in the index.html as the default document. Copy the URL shown up for the static site.

![storage static website](https://github.com/ameetkonnur/blogs/raw/master/img/react-7.gif)

On the storage account go to the "blob" option and you will see a container with the name "$web". You need copy the content of the build folder from step 4 to this container.

![copy file to $web folder](https://github.com/ameetkonnur/blogs/raw/master/img/react-8.gif)

Go to your browser and navigate the URL copied earlier and voila !! you have your website running.

- Step 7

Let's take it notch up. Let's add a text box and take some input and call a backend API based on that input and display the result back. This what most Web Applications do. Usually this is done with a combination of Client side and Server side code but since we dont have a web server here, we dont have a Server side. That's where React comes in and saves the day. React has a beautiful framework to make dynamic single page websites (SPA) that can interact with backend systems and make what looks a static website with JS, HTML & CSS look dynamic.

In this case i have taken a simple use case where a user enters a Github username and the App fetches the basic information about the user and displays this on the page.

![Github User Info App](https://github.com/ameetkonnur/blogs/raw/master/img/react-9.gif)

I will not go into explaining the code for the above App. Once you play around with React a bit, you will understand it.

Enjoy! 

:+1:

# Reference

Github User Info App Source Code : <https://github.com/ameetkonnur/github-user-info-react-app>

React.js Basics : <https://reactjs.org/docs/getting-started.html>

React.js Pluralsight course : <https://www.pluralsight.com/paths/react>






