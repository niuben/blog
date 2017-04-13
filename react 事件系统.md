---
layout: post
title: react事件体系
date: 2017-03-21 19:10 
tags: css 
comments: true
---

react集成了JavaScript事件，并加入一些自己东西变成合成事件。
## 事件绑定
react事件绑定方式js绑定差不多有两种方式：
* 通过标签属性;
* 通过`addEventLister`函数

### 通过标签属性
比如给`<p>'标签绑定`onClick`事件
```
<p onClick={(e)=>{
  console.log(e)
}>
```
<!-- more -->
### 通过`addEventLister`函数
DOM节点有个P标签，
```
<p className="father">father</p>
```
在`componentDidMount`生命周期中绑定
```
document.getElementsByClassName("fater")[0].addEventListener("click", function(e){
  console.log(e);
});
```

## 事件属性
React为事件封装一些属性，[点击查看](https://github.com/niuben/docs/blob/master/web/react.md#事件系统)

## 事件触发流程
React事件冒泡方式和JavaScript完全相同。



