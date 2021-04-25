---
title: 实现SparrowRecsys
date: 2021-02-24 12:06:12
tags:
---

本文章参考极客时间的《深度学习推荐系统实战》

## 介绍

从零开始搭建一个深度学习推荐系统并且从源码了解项目构成

分为几个阶段

1. 基础架构篇
   1. SparrowRecSys 的主要功能和技术架构
   2. 代码研究
2. 特征工程篇
   1. 特征
   2. 特征处理
   3. Spark，Graph Embedding
3. 线上服务篇
   1. Demo 上线，从头到尾打造
4. 推荐模型篇
   1. Embedding + MLP, Wide & Deep, PNN 等深度模型架构和 TensorFlow 实现，以及注意力机制，序列模型，增强学习等相关领域前沿进展
5. 效果评估篇
   1. 线下评估
   2. 线上 AB 测试
   3. 评估反馈闭环
6. 前沿拓展篇
   1. 前沿学习

成果如下图：

![Image of flow](flow.jpg)

这门课主要是为了查漏补缺并且给你一个基本面的概括，包括了 recommendation engine 的知识，TensorFlow 和 Spark 还有其他。

##
