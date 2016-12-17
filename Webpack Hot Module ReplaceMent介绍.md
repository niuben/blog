---
layout: post
title: Webpack Hot Module ReplaceMent介绍
date: 2016/5/3 20:46:25
tags: webpack 
comments: true
---

## Hot Module ReplaceMent介绍
Webpack的Hot Module ReplaceMent（后面简称HMR）是一个很酷的功能。它可以在不刷新页面情况下，实时更新修改过模块。这在开发中带来极大的便利。特别对模块调用关系复杂、层级比较深的WebApp。因为有些模块需要经过多次交互才能展示出来，有了HMR功能后修改代码不用再重新操作一遍。如果想使用上这个HMR功能，需要使用webpack-dev-server调试服务器和在代码中加上module.hot.accept等逻辑。下面具体说一下。

## webpack-dev-server按照
webpack-dev-server是用于webpack项目调试小型静态服务器。具体监听文件变化自动刷新、代理路径、热替换等功能；  
* 安装命令：`npm install -g webpack-dev-server`  
* 启动命令：`webpack-dev-server`  
* 支持HMR功能启动命令：`webpack-dev-server --hot`  
另一种支持HMR功能方式是在webpack.config.js中加入HotModuleReplacementPlugin插件；

## HMR运行的原理  
![webpack官方文档上的图](http://webpack.github.io/assets/HMR.svg)  
上面这张图是引用Webpack官方文档中的图。每个圆形代表是一个代码模块module、圆角长方形代表是代码块（几个module组合而成）。当4和10两个模块编辑更新后，它们的上层或者说是父节点都重新更新，它们下层或者子节点都没有更新；  
从这个例子说明: **模块的更新状态是通过冒泡方式向上传递的；**   
通过每个节点都可以捕获和处理到从子节点冒泡上来更新状态，如果节点都不处理更新状态的话，最终会冒泡到根节点。**如果根节点捕获到更新状态会触发页面刷新**

## 处理更新状态
主要用到module.hot属性，处理方式是在每个模块下加入
```js
if(module.hot) {  
    module.hot.accept();
}
```
这个代码作用是：接受模块更新状态，阻止更新状态向上传递到根节点，从而实现不刷新页面使代码更新的功能；

## demo 
#### main.js
```js
require("./hotA.js");
```

#### hotA.js
```js
alert("hotA");
require("./hotB.js");
```

#### hotB.js
```js
alert("hotB");
require("./hotA.js");
```

使用命令`webpack-dev-server --hot`启动服务器，会分别弹出hotA和hotB两个弹出框。  
当把hotB.js的`alert("hotB")`改成`alert("hotB update")`,整个页面就会重新刷新。  
**如果hotB.js文件改成：**
```js
if(module.hot) {
    module.hot.accept();
 }
alert("hotB update");
require("./hotA.js");
```
页面不会刷新，只有hotB文件重新触发一次。从此调试变得更轻松~


