---
layout: post
title: Babel工作原理
date: 2017/03/5 21:50
tags: babel 
comments: true
---
Babel可以让我们现在就使用最新版本Javascript。它会将最新版本Javascript编译成目前浏览器可以识别的版本。那它是怎么工作呢？

## 名词介绍
介绍Babel工作原理之前需要先了解代码解析中一些名词的含义。

#### 词法分析
将字符序列转换为标记（token）序列的过程。这是代码解析的第一步，举个例子:
```
var n = 1;
```
上面代码经过词法解析生成下面序列
```
"tokens": [
    { "type": {....}, "value": "var", "start": 0, "end": 3, "loc": {...}},
    { "type": {....}, "value": "n:, "start": 4, "end": 5, "loc": {...}},
    { "type": {....}, "value": "=", "start": 6, "end": 7, "loc": {...}},
    { "type": {....}, "value":"1, "start": 8, "end": 9, "loc": {...}},
    .....
]
```
token序列保存每字符的类型、值、开始和结束位置，开始和结束行数和列数。将代码序列化为下一步语法分析做好准备。

#### 语法分析
给token序列中每个字符加上对应的语法类型，序列也从水平结构变成树状结构。比如上面`var`表达式对应的是`VariableDeclaration`类型, 它包含着`declarations`, `kind`等属性;
变量名`n`对应的`Identifier`类型，赋值`1`对应`NumericLiteral`类型。整体语法分析结构为
```
{   
    "type": "VariableDeclaration",        
    "declarations": [
      {
        "type": "VariableDeclarator",
        ....
        "id": {
            "type": "Identifier",
            ...
            "name": "n"
          },
          "name": "n"
        },
        "init": {
          "type": "NumericLiteral",
          ....
          "value": 1
        }
      }
    ],
    "kind": "var"
}
```
经过词法分析和语法分析两个步骤得到结果就是__AST(抽象语法树）__。每个对象称为一个节点。 __Paths__存放节点之间联系，包括状态和作用域等。

#### Visitor(访问者模式)
`Visitor`定义一个作用于对象结构中各类型节点的操作。当遍历对象结构时会调用`visitor`的操作方法最终实现对代码的处理。比如为上面代码的`AST`定义一个`Visitor`

```
    var Visitor = {
        Identifier(path) {
            console.log(path.node.name)  
        }
    }
```
节点语法类型为`Identifier`时会进入该方法并打印出元素。


## Babel主要模块

#### Babylon


#### babel-traverse

#### babel-types

#### babel-generator

## Babel工作流程



