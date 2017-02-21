---
layout: post
title: webpack 防止入口HTML中静态资源缓存
date: 2017/02/19 16:33
tags: webpack2  
comments: true
---
webpack对bundle文件有很好的`md5`支持，可以通过`hash`和`chunkhash`来实现，但对于入口`main.html`中的静态资源md5没有默认的支持，需要配置相关插件来实现。

关于入口HTML中静态资源有三个问题需要解决
* 怎么让入口HTML动态引入`md5`的bundle文件；
* 入口HTML文件中静态资源怎么防止缓存，例如：link引入reset.css情况
* 如果入口HTML文件是前端模板，怎么支持？

### 将动态将bundle文件引入到html中
使用webpack官方插件html-webpack-plugin，可以将打包的bundle文件放入到指定的模板中。
参数如下：
* title: 设置html文件的title
* filename: 生成的文件名。默认是index.html，也可以设置路径。如`/asset/index`
* template: 模板路径；
* inject: true | 'head' | 'body' | false 是否插入bundle文件，如果值是`true`或者`body`会将js bundle文件放在body下面。如果设置为`head`的话则将bundle文件放在`head`中。
* favicon: 添加`favicon`路径；
* minify: {...} | false 通过传递`html-minifier`选项来压缩html
* hash: true | false 设置为`true`将文件中包含所有`scripts`和`css`文件设置一个`hash`，这对防止缓存很有用。
* cache: true | false 设置为`true`只有文件发生改变才再次编译。
* showErrors: true | false 默认为`true`，会将错误信息写到页面中。
* chunks: 允许你添加指定的`chunks`。
* chunksSortMode: 'none' | 'auto' | 'dependency' | * {function} 默认值是`auto` 控制`chunks`的顺序；
* excludeChunks: 制定排除一些`chunks`
* xhtml: true | false If true render the link tags as self-closing, XHTML compliant. Default is false

使用下面配置
```js
//webpack.config.js
....
plugin: [
    new htmlwebpackplugin({
        filename: "./app.html",
        template: './app.html'
    })
]
....
```

### 对html中静态资源支持
模块文件中可能会有`link`，例如`<link href='./reset.css' />`。也需要给`reset.css`文件做缓存处理。
可以使用`html-string-replace-webpack-plugin`插件：可以通过正则匹配的方式将静态资源进行md5. 参数如下：
* `enable`: `true | false` 是否开启；
* `pattern`：创建替换的规则和如何匹配；
* `patterns[parrern].replacement`: 标准的替换函数或者字符串；

```js
new StringHtmlStringReplace({
    enable: true,
    patterns: [
        {            
            // <link href="build.css">  =>
            // <link href="//cdn.baidu.com/static/build.css"> 
            match: /href=\"([^\"]*)\"/g,
            replacement: function (match, $1) {
                return 'href="' + $1 + new Date().getTime()  + '"';
            }
        },
        {
         
            // <script src="build.js">  =>
            // <script src="//cdn.baidu.com/static/build.js"> 
            match: /src=\"([^\"]*)\"/g,
            replacement: 'href="' + '$1"' + new Date().getTime()
    ]
})
```
它会html文件`href`和`src`属性进行匹配。

### 前端模板格式支持
如果使用前端模板，通过对应的loader和`html-webpack-plugin`配合使用。`loader`是对模板的解析成`html`,`html-webpack-plugin`再对`html`进行`md5`处理。
例如对`jade`模板的处理

```js
.....
{
  module: {
    loaders: [
      {
        test: /\.jade$/,
        loader: 'jade-loader'
      },
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'src/index.jade'
    })
  ]
}
......
```

### 总结
`html-webpack-plugin`对html中静态资源处理非常有用。有了这个插件后，就不需要在使用`grunt|gulp`等工具进行处理。

