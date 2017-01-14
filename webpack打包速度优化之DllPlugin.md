---
layout: post
title: webpack打包速度优化之DllPlugin
date: 2016/12/25 19:33
tags: webpack 
comments: true
---
## 遇到问题


　随着web应用化和复杂化，前端开发越来越多依赖第三方文件，比如React、Bootstrap、Moment等。前端加载资源文件数量也极具上升。举个例子，如果你只想用React在页面写个"Hello World"，也要引用大约160个文件(React加上ReactDOM)。一般应用还用一些类似Bootstrap等 UI框架，这样的话相关依赖资源很快都能到达一百多个。例如我目前开发一个后台系统：前端框架用是React，UI库是公司的Ant，光两个依赖的资源数已经达到160多个了。


　这种技术选型在开发中产生一个问题：打包速度慢。做了一个时间测试，用React和Ant新建一个空项目。在没有业务逻辑前提下，通过watch监听文件变化，从文件保存、开始打包到打包结束，整个过程已经耗时大约1.5秒。当业务逻辑不断增加和更多第三方库被引入进来，这个“1.5秒”时间肯定会变大。

　打包速度慢直接影响到开发效率。当只改了一处文案需要等待几秒钟才能在浏览器看到效果，这个开发体验肯定会人难以忍受。项目上线所需时间也会因为打包速度慢而变得更长。分析上面情况后会发现webpack打包速度慢，最主要的原因是第三方文件本身依赖文件太多。时间都消耗在处理第三方文件自身依赖关系上，而业务文件消耗的时间其实很短。想个办法去改进一下。

## 解决方案

　__可以将第三方文件单独打包。__因为第三方文件基本不会改变，所以只需要打包一次。只有在第三方文件发生改变的情况下，比如删除或者增加一个文件，才需要重新进行打包。这里打包生成的文件有点像一个lib库文件，只是它由很多第三方库文件组成。最后将打包生成的文件通过 script 标签、Commonjs、AMD等方式引用进来。script标签引入代码：

```
<script type="text/javascript" src="./dist/lib.bundle.js"></script>
```

　解决第三方文件后，开发过程中只需要打包业务文件。通常业务文件不会像第三方库有那么多依赖关系，引用也相对简单，所以当只打包业务文件的时候，速度会有大大提升。还是用上面”Hello world"为例，我们先把React和Antd单独打包成"lib.bundle.js",然后用script标签方式引用；然后只打包业务文件。

　结果：发现整个打包过程仅用时__”50毫秒“__提高__30倍__速度。打包时间大大的缩短。

## 插件介绍
整个过程需要用到Webpack两个插件。

#### DllPlugin和DllReferencePlugin。
* DllPlugin: 将指定文件和它们所依赖文件生成一张文件id和文件路径映射表。文件id是从入口文件开始按文件被引用顺序生成的。
* DllReferencePlugin: 根据DllPlugin生成映射表，业务文件会找到引用第三方文件id。直接引入文件id号。

　打包速度加快原因是业务文件文件只引用文件ID，不用引用文件内容。这样就不会将第三库文件内容加载到业务文件bundle中，最后生成体积也大大减小。但是整体文件体积没有减小，不过因为第三方文件不会经常改变，所以可以缓存在用户浏览器中。对经常用产品的用户打开页面速度也会大大加快。

　这两个插件使用是有先后顺序，必须先使用DllPlugin然后才能使用DllReferencePlugin。当DllPlugin已经生成映射表(manifes.json)，以后就可以直接使用DllReferencePlugin。只有当需要更新库文件才使用DllPlugin。

#### DllPlugin参数
* path: 映射表生成路径；
* name: bundle文件引用变量名；
<<<<<<< Updated upstream
* context: 设置请求上下文， 默认为webpack.config.js所在路径。

#### DllReferencePlugin参数
* context: 设置映射表里请求的上下文；
* scope: 给引用文件增加前缀；
* manifest: 引用一个DllPlugin生成映射表;
* name: 映射表对应bundle的变量名；
* sourceType: 规定引用模块的方式：比如var,commonjs,amd等，默认是var；
=======
* context: 设文件相对路径，默认为webpack.config.js所在路径。

#### DllReferencePlugin参数
* context: 设文件相对路径，默认为webpack.config.js所在路径；
* scope: 
* manifest: 引用一个DllPlugin生成映射表；
* name: 映射表对应bundle的变量名；
* sourceType: 命名变量名方式； 
>>>>>>> Stashed changes
* content: 映射表内容，默认是指manifest.content。

#### 映射表manifest.json内容如下
```
{
	"name": "lib",
	"content": {
	    "../node_modules/react/react.js": 1,
	    "../node_modules/react/lib/React.js": 2,
	    "../node_modules/react/lib/ReactDOM.js": 3,
	    "../node_modules/react/lib/ReactCurrentOwner.js": 5,
	    "../node_modules/react/lib/ReactDOMTextComponent.js": 6,
	    "../node_modules/react/lib/DOMChildrenOperations.js": 7,
	    "../node_modules/react/lib/Danger.js": 8,
	    ....
	}
}

```
name是bundle变量名，content是文件路径和文件ID映射关系。

## webpack配置
需要两个webpack配置文件： webpack.lib.config.js和webpack.config.js。第一个用于打第三方文件，第二个打包业务文件。一般只会用到一次webpack.lib.config.js文件，除非第三方文件发生了改变。

#### webpack.lib.config.js配置
```js
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
        filename: "[name].bundle.js",
        library: "[name]"
    },
    plugins: [    
        new webpack.DllPlugin({
            path: path.join(__dirname, "dist", "[name].manifest.json"),
            name: "[name]",
            context: "./dist/"
        })
    ]
}
module.exports = config;
```
启动第三方文件打包命令:  ```webpack --config webpack.lib.config.js```

#### webpack.config.js配置
```js
var path = require("path");
var webpack = require("webpack");

var config = {
    entry: {        
        app: ['./index']
    },
    output: {
        path: path.join(__dirname) + "/dist/",
        filename: "[name].bundle.js"
    },
    module: {
        loaders: [{
            test: /\.(js)$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            query: {
                presets: ['react', 'es2015']             
            }
        }]
    },
    plugins: [
        new webpack.DllReferencePlugin({
            context: ".",
            manifest: require("./dist/lib.manifest.json"),          
        })
    ]
}
module.exports = config;
```
启动业务文件打包命令: ```webpack```

## 适用场景
　这种分开打包方式特别适合这种场景：第三方文件体积很大内部依赖资源很多，同时修改频率很低。一些SPA应用业务复杂，需要引入很多第三方文件进行开发。可以考虑这种分开打包的方案。












