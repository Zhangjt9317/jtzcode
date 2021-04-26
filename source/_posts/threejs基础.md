---
title: threejs基础
date: 2021-04-26 16:46:14
tags:
---

本文内容可以与"Three-js 实战与应用"结合，本文更重视基础与概念，帮助你更好的理解 threejs 与它的特性

## Three.js 先决条件

可参考：https://threejsfundamentals.org/threejs/lessons/zh_cn/threejs-prerequisites.html

- 了解闭包
- 了解`this`的工作原理
- ES5/ES6/ES7 特性

  - 使用 for of 而不是 for in
  - 不使用 var
  - 使用解构赋值
  - 使用 `forEach`，`map` 和 `filter` 等
  - 使用对象声明简写
  - 使用扩展运算符`...`
  - 使用 `class`
  - 理解 `getters` 和 `setters`
  - 合理使用箭头函数
  - Promises 以及 async/await
  - 使用模板字符串

- JS 代码风格
- VScode
- 如果你确实需要支持老的浏览器请使用编译器

## Three.js 基础

可参考：https://threejsfundamentals.org/threejs/lessons/zh_cn/threejs-fundamentals.html

Three.js 经常会和 WebGL 搞混，three.js 在 webGL 之上，使用了 WebGL 来绘制三维效果。WebGL 是一个只能画点、线和三角形的非常底层的系统. 想要用 WebGL 来做一些实用的东西通常需要大量的代码， 这就是 Three.js 的用武之地。它封装了诸如场景、灯光、阴影、材质、贴图、空间运算等一系列功能，让你不必要再从底层 WebGL 开始写起。

在我们开始前，让我们试着让你了解一下一个 three.js 应用的整体结构。一个 three.js 应用需要创建很多对象，并且将他们关联在一起。下图是一个基础的 three.js 应用结构。

![threejs_structure](threejs_structure.svg)

上图需要注意的事项：

- 首先有一个渲染器（Renderer）。它是 threejs 的主要对象，你传入了一个场景（Scene）和一个摄像机（Camera）到渲染器（Renderer）中，然后它会将摄像机视锥体中的三维场景渲染成一个二维图片显示在画布上
- 其次有一个场景图（Scene）。它是一个树状结构，有很多对象组成，比如一个图中包含了一个场景（Scene）对象，多个网格（Mesh）对象，光源（Light）对象，群组（Group），三维物体（Object3D），和摄像机（Camera）对象。一个场景（Scene）顶一个场景图最基本的要素，并且包含了背景色和雾等属性。这些对象通过一个层级关系明确的树状结构来展示出各自的位置和方向。子对象的位置和方向总是相对于父对象而言的。比如说汽车的轮子是汽车的子对象，这样移动和定位汽车时就会自动移动轮子。
- 摄像机（Camera）是一般在场景图中，一般在场景图外的。这表示在 threejs 中，摄像机（Camera）和其它对象不同的是，它不一定要在场景图中才能起作用。相同的是，摄像机（Camera）作为其他对象的子对象，同样会继承父对象的位置和朝向。在场景图中有防止多个摄像机在一个场景图中的例子。
- 网格（Mesh）对象可以理解为用一种特定的材质（Material）来绘制的一个特定的集合体（Geometry）。材质（Material）和几何体（Geometry）可以被多个网格（Mesh）对象使用
- 集合体（Geometry）对象代表一些集合体，如球体，立方体，平面，狗，猫，人，建筑等等物体的顶点信息。Three.js 内置了许多基本几何体，你也可以创建自定义的集合体或者从文件中加载几何体。
- 材质（Material）对象代表绘制几何体的表面属性，包括使用的颜色，亮度。一个材质（Material）可以引用一个或者多个纹理（Texture），这些纹理可以用来，打个比方，将图像包裹到几何体的表面。
- 纹理（Texture）对象通常表示一幅要么从文件中加载，要么在画布上生成，要么有另一个场景渲染出的图像
- 光源（Light）对象代表不同种类的光。

有了基本概念以后，画一个如下图所示的 “Hello Cube”吧

![hello_cube](hello_cube.svg)

首先是加载 three.js

```javascript
<script type="module">
  import * as THREE from './resources/threejs/r127/build/three.module.js';
</script>
```

把`type="module"`放到 script 中很重要，这可以让我们使用`import`关键字加载 three.js。其他方法也可以，但是自 r106 开始，使用模块是最推荐的方式。模块的优点是可以很方便地导入需要的其他模块。这样我们就不用再手动引入它们所依赖的其他文件了。

然后我么需要一个`canvas`标签

```javascript
<body>
  <canvas id="c"></canvas>
</body>
```

Three.js 需要使用这个 canvas 标签来绘制，所以我们要先获取它然后传给 three.js

```javascript
<script type="module">
import * as THREE from './resources/threejs/r127/build/three.module.js';

function main() {
  const canvas = document.querySelector('#c'); // css selector refs to canvas id c
  const renderer = new THREE.WebGLRenderer({canvas});
  ...
</script>
```

拿到了 canvas 后我们需要创造一个 WebGL 渲染器（WebGLRenderer）。渲染器负责将你提供的所有数据渲染绘制到 canvas 上。

接下来我们需要一个透视摄像机（PerspectiveCamera）

```javascript
const fov = 75;
const aspect = 2; // 相机默认值
const near = 0.1;
const far = 5;
const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
```

`fov`是视野范围（field of view）的缩写。上述代码中是指垂直方向为 75 度。three.js 中大多数的角用弧度表示，也有某些原因透视摄像机使用角度表示。

`aspect`指画布的宽高比。

![camera_params](camera_params.svg)

```html
<canvas id="c"></canvas>
```

```javascript
import * as THREE from "https://threejsfundamentals.org/threejs/resources/threejs/r127/build/three.module.js";

function main() {
  const canvas = document.querySelector("#c");
  const renderer = new THREE.WebGLRenderer({ canvas });

  const fov = 75;
  const aspect = 2; // the canvas default
  const near = 0.1;
  const far = 5;
  const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
  camera.position.z = 2;

  const scene = new THREE.Scene();

  const boxWidth = 1;
  const boxHeight = 1;
  const boxDepth = 1;
  const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);

  const material = new THREE.MeshBasicMaterial({ color: 0x44aa88 }); // greenish blue

  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  function render(time) {
    time *= 0.001; // convert time to seconds

    cube.rotation.x = time;
    cube.rotation.y = time;

    renderer.render(scene, camera);

    requestAnimationFrame(render);
  }
  requestAnimationFrame(render);
}

main();
```

然后添加一些光照效果更加凸显三维的质感

```javascript
{
  const color = 0xffffff;
  const intensity = 1;
  const light = new THREE.DirectionalLight(color, intensity);
  light.position.set(-1, 2, 4);
  scene.add(light);
}
```

![hello_cube_light](new_hello_cube_light.svg)
