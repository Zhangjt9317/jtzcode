---
title: Basics of Docker
date: 2020-09-23 23:17:28
tags:
---

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


## Images and Containers

1. Docker Image 
   * Image is a template for creating an environment of your choice 
   * Snapshot ==> multiple version of snapshots, and you can point to whichever version you need 
   * Has everything need to run your apps
   * OS, software, app code 

2. Docker Container
  * Running instance of an image 
  * For example ==> Running a redis container, Ngnix container, ....

Example (Ngnix Container):

```
docker pull ngnix 
docker images 
```

now you can run a Ngnix container

```
docker run ngnix:latest
```

you can probably see the process hiding, you can open a new terminal. 

```
docker container ls
```

now you can see the `ngnix:latest` shown

Now you can return to the previous tab and `ctrl c`. run `docker container ls` and nothing is running right now. 

You can also run in a detached mode by running 

```
docker run -d ngnix:latest
docker container ls
```
you can see the running container, in this case you do not have to `ctrl c` or go to another terminal.


## Use of Container

1. Exposing Port 
2. 


## Dockerfile 

* Build you own images 


check the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) if you want to know more.

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



## References:

[1]. https://www.youtube.com/watch?v=t8GbPocwQW0