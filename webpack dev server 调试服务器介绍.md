---
layout: post
title: webpack dev server 调试服务器介绍
date: 2016/5/3 20:46:25
tags: webpack 
comments: true
---


webpack-dev-server是小型静态node服务器，有点像grunt打包工具的grunt server服务器。它主要功能调试一些静态文件，不支持后端语言。webpack-dev-server会将被访问的文件放在内存中，所以webpack-dev-server没有自己的文件夹。如果编译的文件路径和内存中的路径相同，优先调用内存中的文件。

webpack-dev-server主要有下面几个功能：
1. --Content-Base 设置资源目录 
2. Automatic Refresh 自动刷新 
3. Hot Module Replacement 热替换 4. Proxy 代理

## Content-Base 设置资源目录

一般都在webpack.config.js所在目录中，直接Webpack-dev-server命令启动服务器就可以了。有时候会有设计特定路径的要求，比如不在根目录上运行，需要在Grunt打包的dest目录下运行，可以运行：Webpack-dev-server --content-base ./dest
使用了--content-base属性，webpack.config.js的output配置需要增加publicPath属性，如下：
```js
  output: {
    path: "/dest/",
    publicPath: "/dest/"
  }
```

publicPath和path属性的区别是，publicPath是Url，path只是一个文件路径。publicPath比path适用的范围更广。

## Automatic Refresh 自动刷新

webpack-der-server可以监听文件变化状态；当文件变化时自动重新编译文件并刷新浏览器。有两种模式选择： 1. iframe mode： 使用iframe加载页面，并控制和刷新页面； 2. inline mode： 将控制代码加在打包代码中，当代码发生改变页面会刷新；

## iframe mode

使用iframe mode不需要额外配置，直接webpack-dev-server，访问格式是：http://<host>:<port>/webpack-dev-server/<path>

## inline mode

使用inline mode则使用webpack-dev-server --inline，访问格式是http://<host>:<port>/<path>

## Hot Module Replacement 热替换

iframe mode和inline mode都支持Hot Module Replacement功能，使用webpack-dev-server --hot启动服务器

## Proxy 代理

可以在webpack.config.js中配置devServer属性，将Proxy代码加在devServer属性中；