---
layout: post
title: Extract text webpack plugin插件使用详情
date: 2016/5/3 20:46:25
tags: webpack 
comments: true
---
Extract-text-webpack-plugin插件能从webpack打包文件中提取内容并变成文件。

## 优点

* 有更少的style标签
* css具体source-map (如果css打包在js文件里是没有source-map)
* css文件能并行请求
* 更快运行时间（更少代码和Dom操作）

## 缺点

* 更多Http请求
* 更长时间打包编译时间
* 更多配置选项
* 没有热替换

<!-- more -->

## 使用

``` npm intall extract-text-webpack-plugin --save-dev ```

* 在webpack.config.js中添加：var ExtractTextPlugin = require("extract-text-webpack-plugin");
* 在plugins添加：new ExtractTextPlugin("[name].css")
* 在module loaders中添加：{ test: /.scss$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader!sass-loader")}





## extract方法使用

``` extract([notExtractLoader], loader, [options]) ```
* notExtractLoader: 指定不提取的loader
* loader：需要提取内容loader
* options
	* publicPath：设置发布路径，这个参数会覆盖output的publicPath参数；这个参数非常有用，可以改变css文件里资源引用路径，当css文件路径发生改变，不会出现资源找不到的情况；

## 遗留问题

Extract-text-webpack-plugin不能和loader同时操作相同后缀名的文件；例如：

```
module: {
    loaders: [

       { test: /.scss$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader!sass-loader", {publicPath: "./"})},
        { test: /.scss/, loader: "style-loader!css-loader!sass-loader"},
    ]
}
```

拓展阅读：

* extract-text-webpack-plugin
* css文件只能合并的问题

