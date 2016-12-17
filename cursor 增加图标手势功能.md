---
layout: post
title: cursor 增加图标手势功能
date: 2013/01/05 19:33
tags: webpack 
comments: true
---

 当我们要改变鼠标的形状，就当要css的cursor属性。由于各个浏览器的差异，我们需要做一些兼容。

``` css 
  chrome: cursor:url(../image/left1.cur),pointer;
  IE:cursor:url(../image/left1.cur)\9;  
```
cur是icon格式。\9是一种兼容的写法。


