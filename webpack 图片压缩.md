---
layout: post
title: webpack 图片压缩
date: 2016/5/3 20:46:25
tags: webpack 
comments: true
---

###  使用img-loader进行图片压缩。img-loader要和url-loader、file-loader或者raw-loader一起使用才有效果。  
img-loader可以对Gif、Png、Jpg和Svg图片进行压缩，各个图片类型对应组件如下：
* gifsicle ：压缩Gif图片
* jpegtran： 压缩Jpg
* opipng：压缩Png
* pngquant：压缩Png
* svgo：压缩Svg图片