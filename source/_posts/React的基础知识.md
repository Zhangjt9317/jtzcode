---
title: React的基础知识
date: 2020-12-08 23:03:24
tags:
---

# Introduction

1. React的syntax
2. React的DOM
   1. 深入理解虚拟DOM和Diff算法



## 1. React Syntax



## 2. Virtual DOM

### 2.1 深入理解虚拟DOM和Diff算法

在React中，virtual DOM 和 Diff算法的结合大大的提高了渲染效率， diff算法由最初的O(n^3)复杂度变成了现在的O(n)，这一节会解释为什么

#### Virtual DOM 和 Diff

虚拟DOM是一种编程概念。在这个概念中，UI以一种虚拟的表现形式被保存在内存中。更像是一种lightweight copy，并同通过如ReactDOM等类库使之与真实的 "DOM" 同步。这一过程叫做协调。(参考[这里](https://zh-hans.reactjs.org/docs/faq-internals.html))

虚拟DOM更像是一种模式，时常与React元素关联在一起，因为他们都是代表了用户界面的对象。

在React中，render执行的结果得到的并不是真正的DOM节点，而是一种VNode，是轻量级的JavaScript对象。比如：

```JavaScript
<div class="box">
    <span>hello world!</span>
</div>
```

这段代码转换为虚拟DOM结构：

```
{
    tag: "div",
    props:{
        class:"box"
    },
    children: [{
        tag: "span",
        props: {},
        children: ["hello world!"]
    }]
}
```

一般的DOM结构：

```JavaScript
<!doctype html>
<html>
  <head>
    <title>My home page</title>
  </head>
  <body>
    <h1>My home page</h1>
    <p>Hello, I am Marijn and this is my home page.</p>
    <p>I also wrote a book! Read it
      <a href="http://eloquentjavascript.net">here</a>.</p>
  </body>
</html>
```

可以被以下的结构图解释

![DOM](html-boxes.png)




#### 什么是Diff算法

diff算法会对比新老虚拟DOM，记录下它们之间的变化，然后将变化的部分更新到视图上。之前的Diff算法讲循环递归每一个节点并且将他们进行对比，复杂度为O(n^3), n是树中节点的总数，于是性能很差。


#### DOM-DIFF的原理


## 3. React 组件 



## 4. React 状态



## 5. React



## 6. React vs. Vue




## Refs

[1]. https://juejin.cn/post/6844904165026562056