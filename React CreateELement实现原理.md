---
layout: post
title: React createELement实现原理
date: 2017/07/23 19:33
tags: React createElement
comments: true
---
`createElement`方法是用于生成`DOM`元素并可以给`DOM`增加属性和事件。

### 参数介绍
`createElement(tag, props, content|createElement `的参量列表
* tag: 标签名
* props: 标签属性
* content | createElement: 标签内容或者子标签。

### JSX转义
通常我们不会直接编写`createElement`,而是通过`babel`将`jsx`转译成`createElement`方法集合。比如下面例子：

```
<p>hello world</p>
```
这段`html`将会被转译下面方法：
```
React.createElement(
  "p",
  null,
  "hello world"
);
```

将有多个标签时:
```
<div>
  <p>hello world</p>
  <p>hello world</p>
</div>
```
将被转译下面：
```
React.createElement(
  "div",
  null,
  React.createElement(
    "p",
    null,
    "hello world"
  ),
  React.createElement(
    "p",
    null,
    "hello world"
  )
);
```
将有多个标签时需要注意两个问题：
1. 只能有一个根节点
2. `createElement`参量个数不限定

### 元素嵌套实现原理

`React`在执行`createElement`时有以下几步

1. 通过`arguments`对象来遍历参量；
2. 从第三个参量开始判断是字符串和还是对象；
3. 如果是字符串则生成文本节点；
4. 如果是对象则存在`children`属性中。
5. 通过递归的方式重复以上动作，最后形成一个链式结构。