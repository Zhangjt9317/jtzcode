---
title: AWS Practice
date: 2021-04-15 23:46:36
tags:
---

## AWS

Amazon Web Service is like another company compare to Amazon, and you can get lots of introduction to it in its official website.

## Tools

In this article I will go over some examples about how to use aws to build full-stack web apps, covering web hosting, storage, serverless api / functions and others. I will be building 2 - 3 apps, one is a to-do list app, another is a blog app with CRUD operations, and the last is the app I was working on -- a financial app that displays cryptocurrency data and its on-chain metrics, and I call it CryptoVis.

The tools we will be using include:

- aws amplify
  - hosting SPA sites or statis sites
  - allow built-in serverless api / functions / storage options, very user-friendly and convienient
- aws beanstalk
  - wanna host an enterprise level full-stack web (node.js-based or other languages)
- S3
  - simple storage solution can be quite flexible and easier to use than you can expect, for example, financial companies can use it to store stock market data, instead of its common uses in storing content data
-

## Tutorial

First, lets follow the tutorial to build something before coming into the real exmaple. ![link here](https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/) The example shows you step-by-step how to build a notesapp using amplify.

Following the aws amplify tutorial, first, you need to install amplify cli (need to get the latest version, otherwise errors show up in the build phase).

### 1. Create React App

```cmd
npx create-react-app amplifyapp
cd amplifyapp
npm start
```

go to the github and set a new github repo (blank, do not fill in anything). copy and paste the instruction for an existing library. Return to the root directory of your amplifyapp and `git add .` and `git commit -am "first msg"` to stage all changes and commit them. Then you paste what you copied from github to the bash / cmd to push changes to github.

### 2. Host on Amplify

go the aws management console

![console](https://d1.awsstatic.com/webteam/getting_started/GSRC%202020%20updates/Front%20End/Front%20End%20AWS%20Console%20Find%20Amplify%20Module%201.47a33baea2346b85a1d4bd157b9df5bebe693c4b.png)

type amplify and click on the `deploy` option, connect to your github and select the project we just pushed. You will need to authenticate via aws admin ui or other options to authenticate. Everything else should be default, save and deploy.

![react app](https://d1.awsstatic.com/webteam/getting_started/GSRC%202020%20updates/Front%20End/Front%20End%20Amplify%20Deploy%20Source%20Module%201.00becc211a8ecd42349cffb87406449074ed2e5c.png)

### 3. Initialize the local app

go to the root directory of your project and install `npm install -g @aws-amplify/cli`, also, if you have not installed amplify cli, you can find that command in the **amplify admin ui**, which should be on the side bar, click into it click on its side bar, lets say, `function`, and you will see its instruction to install amplify cli, pull amplify backend env and others.

If it is your first time configure amplify cli, type `amplify configure` in your command and set it up, the first 3 choices are default to what you have in your project directory, so it should be:

- vscode
- javascript
- react

the others are just default. After everything is set up, you can add the backend portion in the same project directory, be careful on adding options since it is not revertible unless you physically delete them, and that is quite troublesome.

## 1. TODO-app

## 2. Blog-app

In this case I will build a personal blog app using aws amplify to host website and backend development and deployment, s3 as the storage solution.

## 3. CryptoVis-app
