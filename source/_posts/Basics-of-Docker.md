---
title: Basics of Docker
date: 2020-09-23 23:17:28
tags:
---


# Introduction 


## What is Docker and Why using it

Imaging you have a ecommerce website with multiple servers, and after a while you have surging number of visits, especially in afternoon and evening. So you want more servers to help to process these visits.

In that case, you can rent more VMs from AWS or Azure and turn them on and down. But in order to tell the VM to turn on or down you have to manully add orders to the start file, usually `app.js` or `server.js` file. This is very costly and inefficient. 

One major benefit of docker is to provision resources more efficiently. Back to the example,  


## Docker Intallation

For Windows Enterprise or Pro version users, you can install docker desktop directly, and if the Windows version is not the latest, you can update it first. 

Turn on WSL2 and test docker command in Powershell (run as Administrator)


## Using Docker with Linux

For Windows Home user, you can do the following to run docker:

1. update your Windows version based on the instruction
2. install the Linux Subsystem (Debian, Ubuntu, Zorin, Elmentary OS, etc.)


## Container


## How is Docker Running 


## Example

Here is a tutorial for setting up an Python django web app with docker: [recipe-api-app](https://github.com/Zhangjt9317/recipe-app-api)

In order to use docker and deploy to an image in your project, you need to visit [docker hub](https://hub.docker.com/) first.

Inside docker hub, choose the language you want for your project, and in this case, it is `python`.

The docker hub returns a list of tags that is available. Select `python:3.7-alpine`, a lightweight version to use. It will give you 

First, create a file called `Dockerfile` without any extension, and input the following:

```
FROM python:3.7-alpine
MAINTAINER Zhangjt9317.github.com

ENV PYTHONUNBUFFERED 1

COPY ./requirements.txt /requirements.txt
RUN pip install -r /requirements.txt

RUN mkdir /app
WORKDIR /app
COPY ./app /app

RUN adduser -D user
USER user
```

Then, create a file called `docker-compose.yml` and input:

```
version: "3"

services:
  app:
    build:
      context: .
    ports:
      - "8000:8000"
    volumes: 
      - ./app:/app
    command: >
      sh -c "python manage.py runserver 0.0.0.0:8000"
```

it is a command that tells docker compose what you need to do 