---
layout: post
title: Webpack 运行流程
date: 2018/05/03 19:33
tags: Webpack 原理
comments: true
---
Webpack 执行的如下顺序

* 加载配置文件;
* 分析入口文件的相关依赖模块;
* 每个一个模块通过对应的Loader进行处理；
* 相关模块集合生成一个Chunk;
* 最后Chunk生成Asset;


### 多个Loader如何进行传值
有些文件需要多个loader共同处理，例如`scss`文件同时需要`scss-loader`、`style-loader`、`css-loader`三个组件处理; webpack配置如下

```
rules: [{
    test: /\.css?$/,
    loader: "style-loader!css-loader!scss-loader"
}]
```
loader的执行顺序是从右向左执行: scss-loader第一个执行，执行结束后将值传递给css-loader, css-loader将值再处理一次并将结果传递给style-loader;
通过递归的方式实现上面的功能;

### loader-Runner.js