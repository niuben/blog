---
layout: post
title: webpack2新功能之tree-shaking 
date: 2017/02/11 16:33
tags: webpack2  
comments: true
---

## webpack2新功能之tree-shaking 
最近webpack2正式发布，这个大版本历时一年多时间开发终于发布稳定版本。
tree-shaking是webpack2一大亮点，从字面上能猜出大概的意思：摇一摇树很多叶子就会掉下来，__它能删除没有被`import`引用过的代码。__ 不过这个概念却是rollup.js首先提出来。
我们先看看它的效果怎么样？

### 测试tree-shaking效果
使用大家最常用`react-router`模块测试一下。创建main.js文件，引入react-router几个对象。代码如下：
```js
    import { Router, Route, Link, browserHistory } from 'react-router';
```
我们需要使用babel来编译react。react-router默认是ES5版本，需要通过`resole.mainFileds`属性来调用ES6版本。webpack.config.js配置文件如下：
<!-- more -->
```js
var path = require("path");
var webpack = require("webpack");
module.exports = {
    entry: {
        main: ['./main']
    },
    output: {
        path: path.join(__dirname) + "/dist/",
        filename: "[name].bundle.js"
    },
    module: {
        rules: [{
            test: /\.(js)$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            query: {
                presets: ['react']
            }
        }]
    },
    resolve: {      
        mainFields: ["jsnext:main", "main"]
    }
}
```
同时使用webpack2和webpack1两个版本对这段代码进行打包，对比一下最后文件大小。
__使用webpack2版本进行打包结果如下:__
```
Hash: 8ce95ccddcaf8a12428c
Version: webpack 2.2.1
Time: 2289ms
        Asset     Size  Chunks             Chunk Names
main.bundle.js  65.5 kB       0  [emitted]  main
```

__使用webpack1版本进行打包结果如下:__
```
Hash: 0ebab5b941295d6c81b9
Version: webpack 1.12.2
Time: 3793ms
        Asset    Size  Chunks             Chunk Names
main.bundle.js  107 kB       0  [emitted]  main
```
webpack2打包代码大小是65.5KB，webpack1打包代码是107KB。
__tree-shaking让代码减少了大约40%。当第三方模块体积越大，tree-shaking的效果就会越明显.__

### tree-shaking干了什么？
tree-shaking主要做什么事情？举个官方例子具体说明一下。   
maths.js模块具有square和cube两个方法
```js
// This function isn't used anywhere
export function square(x) {
    return x * x;
}

// This function gets included
export function cube(x) {
    return x * x * x;
}
```

main.js只有引入maths.js模块cube方法

```js
import {cube} from './maths.js';
console.log(cube(5)); // 125
```
执行`webpack -p main.js dist.min.js`命令，生成一个dist.min.js文件，打开并解压缩文件。内容如下：

```js
function(e, t, n) {
    "use strict";

    function r(e) {
        return e * e * e
    }
    t.a = r
}
function(e, t, n) {
    "use strict";
    Object.defineProperty(t, "__esModule", {
        value: !0
    });
    var r = n(0);
    console.log(n.i(r.a)(5))
};
```
最终生产代码中maths.js模块只有`cube`方法，删除没有被引用的`square`方法；
__这是基于ES6模块化一个优化功能，使用它前提是你的代码用ES6开发的。__

### 为什么必须是ES6写法
ES6的export和import要求必须在代码顶层，不能嵌套在函数或者条件判断。因为`import`相对于其他代码会优先执行，生成一个引用。
例如下面代码是不允许：
```js
    setTimeout(function(){
        import {cube} from './maths.js';
        console.log(cube(5));
    }, 1000)
    // => error: 'import' and 'export' may only appear at the top level
```
这种设计导致import无法动态加载模块，所有模块加载一开始就是固定的，从而本地构建工具才可以静态分析出模块之间引用关系，删除那些没有引用的代码。__这就是为什么tree-shaking只能用在ES6的原因__

### 使用场景
当项目代码或者引用第三方库文件是ES6开发的都可以使用webpack2的tree-shaking功能。它说不定能让你的项目来一次大瘦身！







