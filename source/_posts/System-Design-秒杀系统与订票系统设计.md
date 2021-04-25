---
title: System Design -- 秒杀系统与订票系统设计
date: 2021-04-21 12:15:54
tags:
---

## 4S 分析法

只是 work solution，并不是 perfect

- Scenario
  - 需要涉及哪些功能，设计的多牛
  - Ask / Features / QPS / DAU / Interfaces
- Service
  - 将大系统产分成小服务
  - Split / Application / Module
- Storage
  - 数据如何存储和访问
  - Schema / Data / SQL / NoSQL / File System
- Scale
  - 解决缺陷，处理可能遇到的问题
  - Sharding / Optimize / Special Case

## 秒杀系统需要解决问题

- 瞬间大流量高并发
  - 服务器，数据库承载的 qps 有限
- 优先库存，不能超卖
  - 库存是有限的
- 黄牛恶意请求
  - 使用脚本模拟购买
- 固定时间开启
  - 时间到了才能购买，提前一秒都不行
- 严格限购
  - 一个用户，只能购买 1 个或者 N 个

## 需求拆解

- 商家
  - 新建秒杀活动
  - 配置秒杀活动
- 用户
  - 商品秒杀页面
  - 购买
  - 下单
  - 付款

## Service

单体架构 or 微服务

