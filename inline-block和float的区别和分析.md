---
layout: post
title: inline-block和float的区别和分析
date: 2013-03-21 19:10 
tags: css 
comments: true
---

这段时间做项目用到了inline-block和float，这两个属性。发现它们还是有很大的区别的。总结了一下

当设置为inline-block，再设置其中元素margin-top的时候，会导致所有的兄弟元素都起这个作用。但是设置为float的话，就没有这个问题。

看看下面的截图：
<!-- more -->
inline-block: ![inline-block截图](http://img.my.csdn.net/uploads/201303/21/1363864430_6259.JPG)
float: ![float截图](http://img.my.csdn.net/uploads/201303/21/1363864490_8104.JPG)              

不知道这是为什么？
