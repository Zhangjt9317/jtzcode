---
title: System Design -- Twitter Search Engine
date: 2021-04-21 12:15:11
tags:
---

## 推特搜索系统

Twitter Search System

hot words:

- keyword search
- 冷热数据分离
- 雪花算法
- 消息队列
- ES-Hadoop
- 读写分离
- ElasticSearch
- Twitter
- kafka
- HDFS......

### Scenario 场景

设计一个推特搜索系统，

- keyword Search
- Relevance Search
- Facets Search

### Service 服务

- index service
- search service
- ranking service

### Storage 存储

了解整体存储架构，了解搜索引擎

1. Twitter 整体存储架构
   1. Redis 集群
      1. 主要用于缓存数据，大约有四万个节点
   2. 文件存储系统
      1. Twitter 自主开发了一个低成本和可扩展的存储系统 Blobstore，主要是存储图片，video 等大数据文件，大概 4 万个节点
   3. Hadoop 集群
      1. 主要用于后台数据处理，分析日志记录，大概一万个节点
   4. MySQL
      1. 持久化存储关系型数据库

**数据库架构**
