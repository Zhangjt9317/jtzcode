---
title: 原生的JavaScript的DOM操作
date: 2020-12-08 22:58:52
tags:
---


## 什么是DOM

DOM 是HTML和XML文档的编程接口。它提供了对文档的结构化的表述，并且定义了一种方式可以使从程序中对该结构进行访问，从而改变文档的结构，样式与内容。DOM将文档解析为一个由节点和对象组成的结构集合。它将web页面和程序语言连接起来。


## 常见的JS原生DOM操作API总结

* 几种对象
  * Node
  * NodeList
  * HTMLCollection
* 节点查找API
* 节点创建API
  * createElement
  * createTextNode
  * createDocumentFragment
* 节点修改API
  * appendChild
  * insertBefore
  * removeChild
  * replaceChild
* 节点关系API
  * 父关系API
  * 子关系API
  * 兄弟关系API
* 元素属性型API
  * setAttribute
  * getAttribute
* 样式相关API
  * 直接修改元素的样式
  * 动态添加样式规则
  * window.getComputeStyle
  * getBoundingClientRect

## Refs

[1]. https://www.cnblogs.com/liuxianan/p/javascript-dom-api.html
[2]. https://harttle.land/2015/10/01/javascript-dom-api.html
[3]. 
