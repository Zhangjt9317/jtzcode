---
title: "Redis: Usage and Design"
date: 2020-10-13 20:11:08
tags:
  - Redis
  - Cache
categories:
  - Database
  - NoSql
---

## Introduction

A brief summary of Redis:

- Redis is an open-source in-memory data structure store, used as a database, cache and message broker.
- Cache is a temporary stroage component area where the data is stored so that in future, data can be served faster

- Redis is a NoSQL database which follows the principle of key-value store

- Redis holds its database entirely in the memory, using the disk only for persistence

- It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs.

- Redis is written in ANSI C, it is fast

- It supports most of langauges

- It works with Linux and Mac, and no official support for Windows (the latest version updated on Win was version 3, which is not recommended to use)

### Installation

Following the tutorial on Docker and Microsoft website to install the proper version of docker for your machine (whether on Linux subsystem or Win10 pro). Make sure WSL2 is turned on and docker runs properly in the command panel.

### Creating, Starting and Ending a Redis Container

After installation of docker, check if you can run docker Redis image, run

`docker run redis`

to check if connections are ready. You can type `ctrl c` to stop the process (note container does not stop, but you do go to another command line)

run `docker ps` to list out containers, and you can see the Redis container that was just created.

run `docker stop <YOUR CONTAINER NAME>` to stop a running container. The container name can be found in `docker ps`.

run `docker ps` to inspect and ensure your container stops running.

This is because each time we run `docker run redis` it runs a new instance of Redis, which is not what we want to do. We want to create 1 container of Redis and use 1 instance throughout a session or a course.

To do so, we are going to create a container image named `rdb` here, and run

`docker run --name rdb -p 6379:6379 redis`

**only once**. the former 6379 is the port of local machine, the later one is the port of docker container, and we map both ports together.

if a container named `rdb` already exists, run

`docker rm rdb`

to delete it.

After running the command, you will see the connection of Redis, run

`ctrl c`

to stop it and open a new command panel and run `docker stop rdb` to pause the container.

The next time you want to start, just run

`docker start -ai rdb`,

do not create a new container and run `docker run --name rdb -p 6379:6379 redis` again.

### Redis Client

To start a Redis client, when you are running `docker start -ai rdb`, you are running the server, and there are 2 options to run the client:

1. run `docker start -ai rdb` to start the server, run `ctrl c` and run `redis-cli` in the new line
2. run `docker start -ai rdb` to start the server, open a new comamnd panel, run `docker exec -it rdb redis-cli`

You will see a new line with portal `127.0.0.1:6397>` show up when including `-it` in the 2nd option, and that means you have successfully access the Redis client.

You can type `exit` to exit the Redis client.

## Design
