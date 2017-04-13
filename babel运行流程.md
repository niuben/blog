---
layout: post
title: Babel运行流程
date: 2017/03/10 21:50
tags: babel 
comments: true
---

本文主要说一些`Babel`内部运行原理，不具体说一些基础知识。想了解`Babel`基础知识请看这两篇优秀的文章：[Babel 用户手册](https://github.com/thejameskyle/babel-handbook/blob/master/translations/zh-Hans/user-handbook.md)和[Babel 插件手册](https://github.com/thejameskyle/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-avoid-traversing-the-ast-as-much-as-possible)

## 运行流程图
`Babel`执行分为三个步骤：编译、处理、生成。
![流程图](Babel运行机制/bable.png)

<!-- more -->
## 编译
编译步骤主要使用`babylon`模块。`babylon`通过词法分析和语法分析两个步骤将代码转化成`AST`。编译代码如下:
```
import * as babylon from "babylon";
const code = `var n = 1;`;
const ast = babylon.parse(code);`
```
转化成`AST`结构如下：
```

//AST
{   
    "type": "VariableDeclaration",        
    "declarations": [{
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
`AST`记录着代码所有信息，是后面两个步骤的重要基础。代码中每个字符都能在`AST`找到它对应的节点。

## 处理
处理是三个步骤中最难得一步，Babel代码转换都是在这步完成。
在处理步骤中`babel-traverse`模块发挥主要作用，它可以遍历`AST`中每个节点，并将当前节点传递给`plugin`进行增删改操作。

```
import traverse from "babel-traverse";
traverse(ast, visitor);
```
### visitor(访问者模式)
`Babel-traverse`处理AST过程中大量使用了`visitor`模式。`visitor`是`GoF`提出的23种设计模式其中一个。主要适用于数据结构固定不变的场景，它的好处是可以将数据结构和对结构的操作分开。比如下面代码：
```    
    var Visitor = {
        Identifier(path) {
            if(path.node.name == "n") {
                path.node.name = "x"
            }
        }
    }
    
```
这块代码是对`Identifier(标识符)`类型进行处理：如果名称为`n`的标识符改成`x`。下面这段代码经过上面`Visitor`处理后，会变成这样：
```
    var n = 1;
//=> var x = 1;
```
如果有多个`n`也都会被修改`x`，例如
```
function square(n) {
     return n * n;
}
//=> function square(x) {
        return x * x;
    }
```
这是因为`babel-traverse`将会每个节点都传递给`Visitor`进行处理，只有类型为`Identifer`的节点才会进入`Identifer`方法中，而`path`参量包含了该节点所有信息，对`path`的处理从而实现对节点操作。需要注意的是：不同类型的节点`path`参数结构和内容都是不一样的。

虽然每个`plugin`功能各不相同，但是都是按照`visitor`模式进行开发的。掌握`visitor`模式对理解`Babel`运行原理和开发新的`plugin`都是至关重要的。

### presets作用
使用`Babel`的同学都知道`.babelrc`配置文件。配置文件中有两个重要属性:`presets`和`plugins`。这两个属性在编译中主要做了哪些事情呢？我们先看看`presets`属性，比如下面配置：`presets`增加`react`值, 从而可以解析`react`代码。

```
//.babelrc
{
    presets: ["react"]
}
```
其实`presets`就是一个固定`plugin`集合，`Babel`会将`preset`的值转换成对应`plugin`集合。这样在使用过程中就不用记住很多`plugin`名称。比如`react`就会转化为下面几个`plugin`
```
//.babelrc
{
    plugins: [
        "babel-plugin-transform-react-jsx",
        "babel-plugin-syntax-jsx",
        "babel-plugin-transform-react-display-name"       
    ]
}
```

### plugin运行规则
模块化设计让`Babel`变得十分灵活和可扩展。每个`plugin`可以单独使用，也可以和其他`plugin`组合运行。当有多个`plugin`时,它们是这样运行的。
![plugin流程图](Babel运行机制/plugins.png)

多个`plugin`是通过串行的方式运行的。`plugin`的运行先后顺序是由`.babelrc`中`plugins`数组来决定的，按照数组排列顺序依次运行。

`Babel`会遍历`AST`节点，将当前节点按照上面顺序传递给每个`plugin`进行处理，直到节点遍历结束。

## 生成
生成步骤比较简单，主要用`babel-generate`模块实现。`babel-generate`模块将处理过的`AST`转化新的代码。

## 总结
`Babel`是通过使用具有`visitor`模式的`plugin`集合按顺序串行处理`AST`来实现代码的转换。

