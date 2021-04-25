---
title: System Design -- Collaborative Work
date: 2021-04-20 21:35:39
tags:
---

本文会详细的介绍一些例子和他们的技术细节

## 协同编辑系统设计

关键词

- websocket
- File System
- Redis
- MySQL
- CRDT
- IoT
- Domain
- JS
- Database

多人在线编辑同一份文档，google docs，石墨文档，Quip，飞书等等

文档编辑的主要功能

- New / Load File
- Edit File
- Save File

协同文档编辑的核心功能

- 协同
- 展示正在编辑的人
- 内容修改者
- 内容锁定

**重点 -- 同步更新**

我们能不能通过 HTTP 来同步更新？

![httplink](http_link.png)

- 服务器压力太
- 实时性不好

## HTTP 1.0 介绍

1. 连接无法重用,即不支持长链接（Long Connection）

   - http 1.0 规定浏览器与服务器保持较短时间的链接，浏览器每次请求都和服务器经过三次握手(Three-way Handshake)和慢启动(Slow Start)，建立一个 TCP 链接，服务器完成请求处理后立即断开 TCP 链接。

2. 线头阻塞 -- Head of Line (HOL) Blocking

   - 请求队列的第一个请求因为服务器正在忙，或者请求格式问题等其他原因，导致后面的请求被阻塞（block），一个连接就是一个单线程处理，服务器只能响应我的一次 request，后面的请求都会被阻塞。

\*\*早期的网络应用都是通过 HTTP1.0 的方式进行交互的，但是现在的网络应用变得比之前更加复杂，需要交互的场景变得很多，客户端和服务端的交互比较频繁

通过 HTTP1.0 这种形式，客户端需要不断地发送请求给服务器，服务器需要不断地去响应请求，这样对于服务端和客户端压力都比较大，而且交互的效率低。

## HTTP 1.1 介绍

1. 支持长连接
   - 一个 TCP 链接可以传送多个 http 请求（Request）和响应（Response），减少了 TCP 建立和关闭链接的消耗。
2. 支持 HTTP 管道（Pipeline）
   - 管道可以让我们把 FIFO 队列从客户端（请求队列 Request Queue）迁移到服务器（响应队列
     Response Queue），即客户端可以并行(parallel)，服务端串行(Serial)。http1.1 允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照
     接收到客户端请求的先后顺序依次回送响应结果

## 传统文件系统时如何查询文件的？

路径 + 文件名

如：http://xxx.com/folder_1/folder_2/……/file_name

是否适用于 web？ --> 不适用捏

如何设计 URL 更加合理？

- 为每一个文件生成一个唯一的 key，用于识别这个文件， key 可以使用 UUID。
- 加上 slugify 字符串作为后缀增强可读性
-
