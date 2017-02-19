---
layout: post
title: webpack2 主要新功能介绍
date: 2017/02/19 16:33
tags: webpack2  
comments: true
---

最近webpack2正式发布，推出一些很不错的新功能。下面具体介绍一下

### 默认支持ES6
webpack2增加对ES6支持。当使用`import`和`export`的写法时，不再需要借助babel进行编译，而且还可以使用另一个新功能`tree-shaking`。

如果正在使用babel将`import`和`export`转化成`require`格式，可以通过修改`.babelrc`的`preset`选项进行关闭。设置如下：
```js
    //.babelrc文件
    {
        "presets": [
            "env", 
            {"modules": false}
        ]
    }
```

### tree-shaking
因为可以静态分析ES6`import`和`export`，所以webpck可以标记没有被`import`使用的代码。webpack在压缩混淆代码时会删除没有使用的代码，从而产生更小的bundle代码。
举个官方例子说明一下：maths.js模块具有square和cube两个方法
```js
// maths.js
export function square(x) {
    return x * x;
}
export function cube(x) {
    return x * x * x;
}
```
main.js只有引入maths.js模块cube方法
```js
//main.js
import {cube} from './maths.js';
console.log(cube(5)); // 125
```
执行`webpack -p main.js dist.min.js`命令，生成一个dist.min.js文件，打开并解压缩文件。内容如下：
```js
//dist.min.js
....
function(e, t, n) {
    "use strict";

    function r(e) {
        return e * e * e
    }
    t.a = r
}
.....
```
最终生产代码中maths.js模块只有`cube`方法，删除没有被调用的`square`方法。

### 将module.loaders改成module.rules
__webpack2在loader的灵活性和配置方面有很大的改进__，包括可以在`option`属性中设置函数、新增条件匹配属性`issuer`、强制webpack对`pre-loader`分析从而减少编译时间的能力和执行`module`一致性等。

下面通过设置webpack2配置来解析`less`文件：
```js
//webpack.config.js
......
module: {
    rules: [
        {
            test: /\.less$/,
            use: [
                'style-loader',
                { loader: 'css-loader', options: { importLoaders: 1 } },
                'less-loader'
            ]
        }
    ]
}
......
```
### Performance Budgets
由于Web应用越来越复杂引入第三方库也越来越多，导致最后bundle文件越来越大。越来越大bundle文件会导致很多问题：请求文件时间变长、解析文件时间变长、页面展示时间变长等等。webpack2推出 Perfocemance Budgets 功能：当bundle文件过大时会显示警告或者错误信息。

可以设置 Performance Budgets 文件大小的最大值。当bundle文件超过最大值后还可以设置出`warning`或者`error`提示。出现`warning`时不影响打包进程，但出现`error`是会立即终止打包进程，所以官方建议在开发环境下设置`warning`提示，而在生产环境下设置成`error`提示。

下面例子设置入口文件和静态资源最大值约为500KB，超过最大值会出`warning`提示：
```js
//webpack.config.js
......
performance: {
    hints: "warning",
    maxEntrypointSize: 500000, //bytes,
    maxAssetSize: 500000 //bytes
}
......
```

### 其他新的功能

* 通过`import()`方法lazy load ES6模块；
* 增强resolve功能；
* UglifyJsPlugin默认开启`sourcemap`；
* 改变`pre-loader`和`post-loader`方式；
* 大大提高了打包的速度;
* [更多新功能......](https://webpack.js.org/guides/migrating/)


### 总结
webpack2相比上一版本有质的变化，打包速度也大大提高。新项目完全可以大胆使用。虽然新版API绝大部分都支持旧版，但也有几个API接口废弃或者改名，老项目如果想从webpack1迁移到webpack2，需要花一些时间进行调试和修改配置文件。  

由于目前很多第三方库ES6版不完善，虽然tree-shaking功能对减小代码体积很有用，但是会让一些第三方库会出现缺失函数名或属性值相关的警告甚至报错，所以不管新项目还是老项目都要谨慎使用tree-shaking功能。

__总之webpack2是非常值得大家一试！__