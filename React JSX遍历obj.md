---
layout: post
title: react JSX遍历obj
date: 2016/5/3 20:46:25
tags: react 
comments: true
---

使用Object.keys方法获取对象所有key，再通过Key获取具体对象的值；

``` js
var list = {
    a: "a",
    b: "b",
    c: "c"
};
  
Object.keys(list).map((name, index) => {
    return({
    	list[name]
    });
})

```
<!-- more -->