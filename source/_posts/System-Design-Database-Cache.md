---
title: System Design -- Database && Cache
date: 2021-04-20 19:34:09
tags:
---

## Syllabus

- 使用 4S 分析法分析用于系统
- 缓存是什么 -- Cache
- 缓存和数据库如何配合 -- Cache && Database
- 登陆系统如何做 Authentication Service
- 好友关系的存储和查询 Friendship Service
- 以 Cassandra 为例子了解 NoSQL 类型数据库
-

## Design User Systen

实现功能包括注册，登录，用户信息查询，好友关系存储

4S：

- Scenario 场景
  - 注册，登录，查询，用户信息修改
    - 那个需求量最大
  - 支持 100M DAU
  - 注册，登录，信息修改 QPS 约
    - 100M \* 0.1 / 86400 ~ 100
    - 0.1 = 平均每个用户每天登陆 + 注册 + 信息修改
    - Peak = 100 \* 3 = 300
  - 查询的 QPS 约
    - 100M \* 100 / 86400 ~ 100k
    - 100 = 平均每个用户与查询用户信息相关的操作次数（查询好友，发信息，更新消息主页）
    - Peak = 100k \* 3 = 300k
- Service 服务
  - 一个 AuthenticationService 负责登陆注册
  - 一个 UserService 负责用户信息存储与查询
  - 一个 FriendshipService 负责好友关系存储

## QPS 与系统设计的关系

为什么要分析 QPS？QPS 的大小决定了数据存储系统的选择

- Storage 存储
  - MySQL / PosgreSQL 等 SQL 数据库的性能
    - 约 1k QPS 这个级别
  - MongoDB / Cassandra 等硬盘型 NoSQL 数据库的性能
    - 约 10k QPS
  - Redis / Memcached, 内存型 NoSQL
    - 100k ~ 1m QPS 这个级别

以上根据机器性能和硬盘数量以及硬盘读写速度会有区别

**思考**

- 注册，登录，信息修改，300QPS，适合什么数据库存储系统？
- 用户信息查询适合什么数据存储系统？

## 用户系统特点

读非常多，写非常少，一个读多写少的系统，一定要使用 Cache 进行优化。

- Cache 是什么

  - 缓存，
  - Java 中的 HashMap
  - key-value 结构

- 有哪些常用的 Cache 系统/软件

  - Memcached(不支持数据持久化)
  - Redis(支持数据持久化)

- Cache 一定是存在于内存中的吗？

  - 不是
  - Cache 这个概念，并没有指定存在什么样的存储介质中
  - File System 也可以作为 Cache
  - CPU 也有 Cache

- Cache 一定指的是 Server Cache 吗？
  - 不是，客户端也有 Cache

## Memcached

一款负责帮助你 Cache 在内存中的“软件”，非常广泛使用的数据存储系统

例子：

if using Python:

```python
from pymemcache.client import base

# Don't forget to run `memcached' before running this next line:
cache = base.Client(('localhost', 11211))

# Once the client is instantiated, you can access the cache:
cache.set('some_key', 'some value')

# Retrieve previously set data again:
cache.get('some_key')

# 'some value'
```

```python
class UserService:

    def getUser(self, user_id):
        key = "user::%s" % user_id
        user = cache.get(key)
        if user:
            return user
        user = database.get(user_id)
        cache.set(key, user)
        return user

    def setUser(self, user):
        key = "user::%s" % user.id
        cache.delete(key)
        database.set(user)
```

以下那些写法是不对的？

A: database.set(user); cache.set(key, user);
B: database.set(user); cache.delete(key);
C: cache.set(key, user); database.set(user);
D: cache.delete(key); database.set(user);

**Answer: None**

第一个如果成功了，第二个操作失败了，都会导致 inconsistency，称之为 Dirty Data

如果第一个失败第二个成功了？

**D: cache.delete(key); database.set(user);**

D 选项中，不会因为第一个操作成功，而第二个操作不成功造成数据不一致。因为 cache.delete 是删
除缓存中的数据，而不是修改缓存中的数据，第二个操作失败以后，信息相当于没有被修改，虽然操
作失败了，但是没有造成缓存与数据库的数据不一致。
但是 D 这个选项仍然存在问题，请问在什么情况下会造成数据不一致？

**多进程模式中**

回到我们的Python程序中，在setUser执行到14 - 15行，另外一个程序进程执行了getUser，此时cache里的数据是旧数据。



## 解决方法可以给数据库