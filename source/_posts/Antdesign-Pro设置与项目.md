---
title: Antdesign Pro设置与项目
date: 2021-04-06 18:08:53
tags:
---

## Antdesign Pro Basics and Layout

## Dva and State Management

## Docker

Ant Design Pro 是一个企业级中后台解决方案，在 Ant Design 组件库的基础上，提炼出典型模板/业务组件/通用页等，在此基础上能够使开发者快速的完成中后台应用的开发。

**为何使用 docker？**

- 环境部署是所有团队都必须面对的问题，随着系统越来越大，依赖的服务也越来越多，例如：Web 服务器 + MySql 数据库 + Redis 缓存等
- 依赖服务很多，本地搭建一套环境成本越来越高，初级人员很难解决环境部署中的一些问题
- 服务的版本差异及 OS 的差异都可能导致线上环境 BUG，项目引入新的服务时所有人的环境需要重新配置

**常见 docker 命令**

- 使用当前目录 Dockerfile 创建镜像，标签为 xxx:v1: docker build -t xxx:v1 .
- 创建新容器并运行: docker run --name mynginx -d nginx:latest
- 在容器中开启交互终端：docker exec -i -t container_id /bin/bash
- 启动容器：docker start container_name/container_id
- 停止容器：docker stop container_name/container_id
- 重启容器：docker restart container_name/container_id


compose中有两个重要的概念：

服务 (service)：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。
项目 (project)：由一组关联的应用容器组成的一个完整业务单元，在docker-compose.yml 文件中定义。