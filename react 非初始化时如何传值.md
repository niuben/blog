---
layout: post
title: react 非初始化时如何传值
date: 2016/5/3 20:46:25
tags: react 
comments: true
---

一般都是通过getInitialState和getInitialProp两个进行初始化传值； 如果遇到值需要异步ajax获取的话，则通过componentDidMount和componentWillMount两个方式。 有一种情况时，组件并非初始化时传值，已经初始化完成了，需要中途给它传值；这种情况下有下面三个方法

* componentWillReceiveProp: 得到新的props会被调用，但是初始化时不会被调用；而且使用setState时不会触发二次渲染；
* componentDidUpdate：用于prop和state已经传递，并且DOM节点已经发生变化；
* componentWillUpdate：用于prop和state已经传递， DOM节点没有发生改变；这里不能使用setState方式

例子中获得classID

```
componentWillReceiveProps(props){
       var tagData = props.tagData;
        if(tagData) {
            this.setState({
                classifyID:  tagData.tagClassifyId
            });
        }
    }
```
<!-- more -->