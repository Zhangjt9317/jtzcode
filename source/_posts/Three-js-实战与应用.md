---
title: Three.js Practice and Examples
date: 2021-04-04 13:33:33
tags:
---

Three.js is very popular now, and the framework is getting pretty stable, good time to learn. Check out these exmaples on the website to see how capable it is.

This article refers to the [Three.js Journey course](https://threejs-journey.xyz/lessons/3)

## 03 | Build a Scene

To actually be able to display anything with three.js, we need three things:

- scene
- camera
- renderer

so that we can render the scene with camera:

```javascript
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="./three.js"></script>
    <script src="./script.js"></script>
</body>
</html>
```

go to the threejs.org to check how to create a scene, first, download / copy the build of three.js [here](https://threejs.org/build/three.js) and past into a js file (called three.js in my folder) in your project, and then refer to it inside `<script></script>` tag in index.html.

inside the script.js created, we will create a box first

```javascript
// create a scene
const scene = new THREE.scene();

// Red cube
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);

// Camera
const camera = new THREE.PerspectiveCamera();
scene.add(camera);
```

## 04 | Webpack

in the previous section we loaded the Three.js in a single file, it has limitations, **it does not include some of the Three.js classes because the file would be too heavy -- in one file** (use the three.min.js).

If you know nothing about webpack, go to the official documentation page and take a look at the tutorial, or you can check my post about Fundamental and Advanced Use of Webpack (webpack 技术总结 --> ctrl f to search).

in this section we are going to need to run a server to load and manipulate images, to do that and because of security reasons, we need to run a local server. --> basically use classes in the three.js

The simlest way of handling those issues is to use a bundler, which involves webpack.

**Note: it is definitely harder to do this locally, try codepen and codesandbox if you can**

**What is a bundler?**

A bundler is a tool in which you send assets like JS, CSS, HTML and other languages and bundle them up into static assets, check webpack's official documentation for that.

the basic folder structure looks like the following:

```markdown
- bundler
  - webpack.common.js
  - webpack.dev.js
  - webpack.prod.js
- dist --> show after build
- src
  - index.html
  - script.js
  - style.css
- static
  - .gitkeep
  - door.jpg
- package-lock.json
- package.json
- readme.md
```

the bundler is provided:

webpack.common.js

```javascript
const CopyWebpackPlugin = require("copy-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCSSExtractPlugin = require("mini-css-extract-plugin");
const path = require("path");

module.exports = {
  entry: path.resolve(__dirname, "../src/script.js"),
  output: {
    filename: "bundle.[contenthash].js",
    path: path.resolve(__dirname, "../dist"),
  },
  devtool: "source-map",
  plugins: [
    new CopyWebpackPlugin({
      patterns: [{ from: path.resolve(__dirname, "../static") }],
    }),
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "../src/index.html"),
      minify: true,
    }),
    new MiniCSSExtractPlugin(),
  ],
  module: {
    rules: [
      // HTML
      {
        test: /\.(html)$/,
        use: ["html-loader"],
      },

      // JS
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ["babel-loader"],
      },

      // CSS
      {
        test: /\.css$/,
        use: [MiniCSSExtractPlugin.loader, "css-loader"],
      },

      // Images
      {
        test: /\.(jpg|png|gif|svg)$/,
        use: [
          {
            loader: "file-loader",
            options: {
              outputPath: "assets/images/",
            },
          },
        ],
      },

      // Fonts
      {
        test: /\.(ttf|eot|woff|woff2)$/,
        use: [
          {
            loader: "file-loader",
            options: {
              outputPath: "assets/fonts/",
            },
          },
        ],
      },
    ],
  },
};
```

webpack.dev.js

```javascript
const { merge } = require("webpack-merge");
const commonConfiguration = require("./webpack.common.js");
const ip = require("internal-ip");
const portFinderSync = require("portfinder-sync");

const infoColor = (_message) => {
  return `\u001b[1m\u001b[34m${_message}\u001b[39m\u001b[22m`;
};

module.exports = merge(commonConfiguration, {
  mode: "development",
  devServer: {
    host: "0.0.0.0",
    port: portFinderSync.getPort(8080),
    contentBase: "./dist",
    watchContentBase: true,
    open: true,
    https: false,
    useLocalIp: true,
    disableHostCheck: true,
    overlay: true,
    noInfo: true,
    after: function (app, server, compiler) {
      const port = server.options.port;
      const https = server.options.https ? "s" : "";
      const localIp = ip.v4.sync();
      const domain1 = `http${https}://${localIp}:${port}`;
      const domain2 = `http${https}://localhost:${port}`;

      console.log(
        `Project running at:\n  - ${infoColor(domain1)}\n  - ${infoColor(
          domain2
        )}`
      );
    },
  },
});
```

webpack.prod.js

```javascript
const { merge } = require("webpack-merge");
const commonConfiguration = require("./webpack.common.js");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = merge(commonConfiguration, {
  mode: "production",
  plugins: [new CleanWebpackPlugin()],
});
```

if you run `npm run build` you will have the `dist` folder with outcome static assets that are ready to be deployed. In the `index.html` file you do not need to add `<script></script>` tag, webpack will add it for you. the `script.js` file and `style.css` should all be in the `/src/` directory.

Run `npm run dev` everytime you want to start coding and see on the screen (nodemon can also be used to watch your actions in real time).

## 05 | Transform Objects

for all objects / classes in the Object3D category has 4 features in three.js

- position --> to move the object
- scale --> to resize the object
- rotation --> to rotate the object
- quaternion --> also rotate the object, expressed in the form **(a + bi + cj + dk)**

**Move Obejcts**

```javascript
mesh.position.x = 0.7;
mesh.position.y = -0.6;
mesh.position.z = 1;
```

![image](https://threejs-journey.xyz/assets/lessons/05/step-02.png)

the `position` property is not any object. It is an instance of the `Vector3` class. while this class has an x, y and z, it has many useful methods.

```javascript
console.log(mesh.position.length()); // get the length of a vector
console.log(mesh.position.distanceTo(camera.position)); // get the distance from another Vector3
console.log(mesh.position.normalize()); // normalize its values (reduce the length to 1 unit but preserve its direction)
mesh.position.set(0.7, -0.6, 1); // to change the values
```

**Axes Helper**

positiong things in space relies on axes, using `AxesHelper` can help you, it will display 3 lines corresponding to x, y, z axes, each one starting at the center of the scene and going in the corresponding direction.

```javascript
const axesHelper = new THREE.AxesHelper(2);
scene.add(axesHelper);
```

![axes helper](axes_helper.png)

the green and red line will show up. the green line --> y axis, the red line --> x axis and there is a blue line --> z axis not shown due to the camera angle.

**Scale Objects**

`scale` is also a `Vector3`, by default, x, y, z are equal to 1, meaning that the object has no scaling applied. If you put 0.5 as a value, the object will be half of its size on this axis, and if you put 2, it will be 2x size.

```javascript
mesh.scale.x = 2;
mesh.scale.y = 0.25;
mesh.scale.z = 0.5;
```

![scale objects](scale_objects.png)

**Rotate Objects**

```javascript
mesh.rotation.x = Math.PI * 0.25;
mesh.rotation.y = Math.PI * 0.25;
```

![rotation](rotation.png)

**Quaternion**

The `quaternion` property also expresses a rotation, but in a more mathmatical way, which solves the order problem.

**Look at this**

Object3D instances have an excellent method named `lookAt(...)` that lets you ask an object to look at something. The object will automatically rotate its `-z` axis toward the target you provided.

```javascript
camera.lookAt(new THREE.Vector3(0, -1, 0));
```

![lookat](lookat.png)

**Combining Transformations**

you can combine the `position`, `rotation`, and `scale` in any order. The result will be the same, its equivalent to the state of the object.

```javascript
mesh.position.x = 0.7;
mesh.position.y = -0.6;
mesh.position.z = 1;
mesh.scale.x = 2;
mesh.scale.y = 0.25;
mesh.scale.z = 0.5;
mesh.rotation.x = Math.PI * 0.25;
mesh.rotation.y = Math.PI * 0.25;
```

![comb](comb.png)

**Scene Graph**

At some point, you might want to group things, lets say you are building a house with walls, doors, windows, a roof, brushes, etc.

When you think you are done, you become aware that the house is too small, and you have to re-scale each object and update their positions.

A good alternative would be to group all those objects into a container and scale that container, and you can do that with the `Group` class.

Instantiate `Group` class and add it to the scene. Now, when you want to create a new object, you can add it to the `Group` you just created using the `add(...)` method rather than adding it directly to the scene.

Since the `Group` class inherits from the `Object3D` class, it has access to the previously-mentioned properties and methods like position, scale, rotation, quaternion, and lookAt.

Comment the `lookAt(...)` call and, instead of our previously created cube, create 3 cubes and add them to a `Group`. Then apply transformations on the group:

```javascript
/**
 * Objects
 */
const group = new THREE.Group();
group.scale.y = 2;
group.rotation.y = 0.2;
scene.add(group);

const cube1 = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
cube1.position.x = -1.5;
group.add(cube1);

const cube2 = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
cube2.position.x = 0;
group.add(cube2);

const cube3 = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
cube3.position.x = 1.5;
group.add(cube3);
```

![scene_graph](scene_graph.png)

## 06 | Animations

The most important step in threejs and other animation engines is to animate your obejcts. We have created a scene that we rendered once at the end of our code. That is already good progress, but most of the time, you will want to animate them.

using animation in three.js works like stop motion. You move the objects, and you do a render. Then you move the objects more, and another render. etc. the more you move the objects between renders, the faster they will appear to move.

## 07 | Cameras

## 08 | Fullscreen and Resizing

## 09 | Geometrics

## 10 | Debug UI

while you can create your own debug UI using HTML/CSS/JS, there are already multiply libraries

- dat.GUI
- control-panel
- ControlKit
- Guify
- Oui

All of these can do what we want, but we will use the most popular one, which is dat.GUI. Feel free to try the other ones.

For example,

![debug_ui](debug_ui.png)

### How to implement Dat.GUI

To add Dat.GUI to our webpack project, we can use npm to install it `npm install --save dat.gui` note that `--save` is necessary if you want that package to be included in your project build.

**note if you wanna delete or add some modules, or the entire node_modules, you can use `npx npkill` to delete everything like a pro**

relaunch the server

```javascript
import "./style.css";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import gasp from "gasp";
import * as dat from "dat.gui";

//........
```

now you can instantiate the Dat.GUI:

```javascript
/**
 * Debug
 */
const gui = new dat.GUI();
```

This results in an empty panel on the top right corner of the screen.

There are different types of elements you can add to that panel:

- Range -- for numbers with minimum and maximum value
- Color -- for colors with various formats
- Text -- for simple texts
- Checkbox -- for booleans ( `true` or `false`)
- Select -- for a choice from a list of values
- Button -- to trigger functions
- Folder -- to organize your panel if you have too many elements

### Add elements

to add an element to the panel, you must use `gui.add(...)`. The first parameter is an object and the second parameter is the property of that object you want to tweak. You need to set it after you created the concerned object:

```javascript
gui.add(mesh.position, "y");

// specify min and max value and the precision
gui.add(mesh.position, "y", -3, 3, 0.01);

// or you can use min(), max(), step() and use add() to chain them
gui.add(mesh.position, "y").min(-3).max(3).step(0.01);

// you can add a label for this
gui.add(mesh.position, "y").min(-3).max(3).step(0.01).name("elevation");
```

Dat.GUI will automatically detect what kind of property you want to tweak and use the corresponding element. A good example is the `visible` property of Object3D. It is a boolean that, if `false`, will hide the object:

```javascript
const parameters = {
  color: 0xff0000,
};
gui.add(mesh, "visible");
gui.add(material, "wireframe");
gui.addColor(parameters, "color").onChange(() => {
  material.color.set(parameters.color);
});
const material = new THREE.MeshBasicMaterial({ color: parameters.color });

// add spin action using gsap ==> need to check the version, otherwise gsap.to does not exist
const parameters = {
  color: 0xff0000,
  spin: () => {
    gsap.to(mesh.rotation, { duration: 1, y: mesh.rotation.y + Math.PI * 2 });
  },
};
gui.add(parameters, "spin");
```

### Tips

### Hide

Press `H` to hide the panel, if you want the panel to be hidden from start, call `gui.hide()` after instantiating it.

### Close

You can close the panel by clicking on its bottom part.

If you want the panel to be closed by default, you can send an object when instantiating Dat.GUI and pass it `closed: true` in its properties:

```javascript
const gui = new dat.GUI({ closed: true });
```

### Width

you can change the panel's width by drag and dropping its left border (although please note, it doesn't always work).

if you want to change the default width of the panel, add `width: ...` in the properties:

```javascript
const gui = new dat.GUI({ width: 400 });
```

if you want to know more about Dat.GUI, there are some links:

- Github repository: https://github.com/dataarts/dat.gui
- API documentation: https://github.com/dataarts/dat.gui/blob/HEAD/API.md
- A simple but complete example: https://jsfiddle.net/ikatyang/182ztwao/

## 11 | Textures

### What are textures?

images that will cover the surface of your geometries, many types of textures can have different effects on the apperances of your geometry. It's not just about the color.

Here are some most common type of textures using a famous door texture by Joao Paulo.

![door_albedo](door_albedo.jpg)

- Color (or albedo): The albedo texture is the most simple one. it only takes the pixels of the texture and apply them to the geometry
- Alpha: The alpha texture is a grayscale image where white will be visible, and black won't
  ![alpha](alpha.jpg)
- Height: The height texture is a grayscale image that will move that vertices to create some relief. You will need to add subdivision if you want to see it
  ![height](height.jpg)

- Normal: The normal texture will add small details. It won't move the vertices, but it will lure the light into thinking that the face is oriented differently. Normal textures are very useful to add details with good performance because you don't need to subdivide the geometry
  ![Normal](Normal.jpg)

- Ambient Occulusion: a grayscale image that will fake shadow in the surface's crevices. While it's not physically accurate, it certainly helps to create contrast
  ![amb_occ](amb_occ.jpg)

- Metalness: a grayscale image that will specify which part is metallic (white) and non-metallic(black). This information will help to create reflection
  ![Metalness](Metalness.jpg)

- Roughness: a grayscale image that comes with metalness, and that will specify which part is rought (white) and which part is smooth (black). This information will help to dissipate the light. A carpet is very rugged, and you won't see the light reflection on it, while the water's surface is very smooth, and you can see the light reflecting on it. Here, the wood is uniform because there is a clear coat on it
  ![Roughness](Roughness.jpg)

### PBR

Those textures (especially the metalness and the roughness) follow what we call PBR principles. PBR stands for **Physically Based Rendering**. It regroups many techniques that tend to follow real-life directions to get realistic results.

There are many other techniques, while PBR is becoming the standard for realistic renders, and many software engines, and libraries are using it.

For now, we will simply focus on how to load textures, how to use them, what transformations we can apply, and how to opitimize them. Here are some refs:

- https://marmoset.co/posts/basic-theory-of-physically-based-rendering/

- https://marmoset.co/posts/physically-based-rendering-and-you-can-too/

### How to load textures

### Getting the URL of the image

To load the texture, we need the url of the image file .

Because we are using webpack, there are two ways of getting it.

You can put the image texture in the `/src` folder and import it like you would import a JS dependency:

```javascript
import imageSource from "./image.png";

console.log(imageSource);
```

or you can put that image in the `/static/` folder and access it just by adding the path of the image (without `/static`) to the URL:

```javascript
const imageSource = "/image.png";

console.log(imageSource);
```

Be careful, this `/static/` folder only works because of the webpack template's configuration. If you are using other types of bundler, you might need to adapt your project.

We will use the `/static/` folder technique for the rest of the course.
