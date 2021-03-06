---
title: 深入剖析k8s
date: 2021-04-03 21:29:06
tags:
---

参考极客时间课程：https://time.geekbang.org/column/article/14252

从过去以物理机和虚拟机为主体的开发运维环境，向以容器为核心的基础设施的转变过程，并不是一次温和的改革，而是涵盖了对网络，存储，调度，操作系统，分布式原理等各个方面的容器化理解的改造。

这导致了很多初学者要么知识储备不足，要么杂乱无章，不成体系，这也是许多初次参与 PaaS 项目的从业者们共同面临的一个困境。

1. 容器技术基础
2. K8S 集群的搭建与实践
3. 容器编排与 K8S 核心特性剖析
4. K8S 开源社区与生态

## 介绍

容器不是一个新鲜的东西，也不是 Docker 公司发明的。即使是当时最热门的 PaaS 项目中，也是在最底层。

PaaS 项目被大家接纳的一个主要原因，是因为它提供了一种叫“应用托管”的能力，那时候虚拟机和云计算已经是普遍的技术和服务了，主流的用法是租用一批 AWS 或者 OpenStack 的虚拟机，然后像以前管理物理服务器那样，用脚本或者手工的方式在机器上部署应用。

由于需要在一个虚拟机上启动很多个来自不同用户的应用，Cloud Foundry 会调用操作系统的 Cgroups 和 Namespace 机制为每一个应用单独创建一个称作“沙盒”的隔离环境，然后在“沙盒”中启动这些应用进程。这样，就实现了把多个用户的应用互不干涉的在虚拟机里批量的，自动的运行起来的目的。

这就是 PaaS 项目的核心能力，而这些 Cloud Foundry 用来运行应用的隔离环境，或者说“沙盒”，就是所谓的容器。

而 Docker 项目，实际上跟 CF 的容器没有太大的区别。Docker 的主要功能，区别于 CF 的，就是**Docker 镜像**。

PaaS 之所以可以帮助用户大规模部署应用到集群里，是因为它提供了一套应用打包的功能，也就是一个被人诟病的软肋。

这个问题的原因是，一旦用上 PaaS，用户就必须为各种语言，每种框架，甚至每个版本的应用维护一个打好的包。这个打包过程没有任何章法可循，更麻烦的是，需要很多配置在 PaaS 中跑起来，而这些配置和修改没什么经验可以借鉴，基本上得靠不断试错。

而 Docker 镜像解决了这个打包的根本性问题。

熟悉 Docker 的话就知道，只需一个下载好的操作系统文件与目录，然后使用它制作一个压缩包即可，就是：

```bash
docker build "myMirror"
```

一旦镜像制作完成，用户就可以让 Docker 创建一个“沙盒”来解压这个镜像，然后再“沙盒”中运行自己的应用，就是

```bash
docker run "myMirror"
```

所以，Docker 项目给 PaaS 世界带来的“降维打击”，其实是提供了一种非常便利的打包机制。这种机制直接打包了应用运行所需要的整个操作系统，从而保证了本地环境和云端环境的高度一致，避免了用户通过“试错”来匹配两种不同运行环境之间差异的痛苦过程。

## 1. 技术剖析

### 从进程说开去

- 容器技术兴起于 PaaS 技术的普及
- Docker 公司发布的 Docker 项目有里程碑式的意义
- Docker 项目通过“容器镜像”解决了打包这个难题

**容器本身没有价值，有价值的事“容器编排”**

也正因为如此，容器技术生态才爆发了一场关于“容器编排”的“战争”。而这次战争，最终以 Kubernetes 项目和 CNCF 社区的胜利而告终。所以，这个专栏后面的内容，我会以 Docker 和 Kubernetes 项目为核心，为你详细介绍容器技术的各项实践与其中的原理。

总结是
![containerImage](container.jpg)

### 隔离与限制

Linux 容器中用来实现”隔离“的技术手段：Namespace。通过这些讲解，
Namespace 技术实际上修改了应用进程看待整个计算机“视图”，即它的“视线”被操作系统做了限制，只能“看到”某些指定的内容。但是对与宿主来说，这些被“隔离”了的进程跟其他进程并没有太大区别。

![containerImage](container.jpg)
