---
layout: post
title: webpack 根据库文件是否更新进行md5
date: 2017/02/19 16:33
tags: webpack2  
comments: true
---
webpack对`bundle`文件的`md5`已经做的很好，但是有种场景的文件md5需要特别注意：对库文件md5。需要满足下面的条件：
* 库文件很大需要分开打包，不然影响业务文件打包速度；
* 库文件是通过`<script>`标签引入的；

如下面代码：
```html
    <script src="./lib.js" type="text/javavscript"></script>
```
`webpack.lib.config.js`单独打包`lib.js`文件
```
var path = require("path");
var webpack = require("webpack");
var config = {
    entry: {
        lib:[
            "react", 
            "react-dom", 
             "antd", 
        ],
    },
    output: {
        path: path.join(__dirname) + "/dist/",        
        filename: "[name].js",
        library: "[name]"
    },
    plugins: [    
        new webpack.DllPlugin({
            path: path.join(__dirname, "dist", "[name].manifest.json"),
            name: "[name]",
            context: "./",            
        })
    ]
}
module.exports = config;
```
通过`webpack --config webpack.lib.config.js`命令可以生成`lib.js`文件

__这里有个问题就是`lib.js`文件没有办法自动加`md5`戳。__

## 通过webpack-manifest-plugin来解决
使用`webpack-manifest-plugin`可以生成每个`bundle`文件对应的`md5`文件名的json文件，如下：
``` 
//manifest.json
{
    "lib.js": "lib.bundle.c3232323232fdc0a83.js"
}
```
在webpack.config.js将`manifest.json`文件引入就可以知道lib.js对应的md5文件名，在通过`html-string-replace-webpack-plguin`插件进行正则匹配就可实现。代码如下：
```
var path = require("path");
var HtmlWebpackPlugin = require("html-webpack-plugin");
var StringReplacePlugin = require("html-string-replace-webpack-plugin");
var ManifestJSON = require('./dist/manifest.json');

var index = 0;
var config = {
    entry: {
        app: ['./app']
    },
    output: {
        path: path.join(__dirname) + "/dist/",
        filename: "[name].bundle.js",
    },
    plugins: [
	    new HtmlWebpackPlugin({
		    filename: "./index.html",
		    template: "./index.html",
	  	}),
        new StringReplacePlugin({
            enable: true,
            patterns: [
                {
                    match: /lib.bundle.js/g,
                    replacement:function(match, url){
                        return ManifestJSON["lib.js"] + "&?=12"
                    }
                }
            ]
        })
	]
}
module.exports = config;
```











