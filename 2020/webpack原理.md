## Webpack原理

* 配置文件解析    
    * entry入口
    * output出口    
    * compiler实例化
* 文件加载
    * loader
* 插件
    * plugin
* 文件编译

### 配置文件解析
`webpack`一开始会加载`webpack.config.js`配置文件，通过解析配置文件找到相关属性。


### 文件加载
`webpack`提供一个`loader`功能，用于加载各种类型的文件。`loader`主要的作用是将文件传递给第三方库，第三方库处理完成后再将内容传回来。  
`webpack.config.js`配置文件使用`use`关键词来设置文件和`loader`之间的调用关系。


### 插件
`webpack`基于`tapable hook`实现了hook功能。
为什么`webpack`要采取`hook`的设计方式，个人觉得有下面几个原因:
1. `hook`可以进行逻辑分解达到分而治之的效果，可以有效降低代码复杂度;
2. `hook`优势在于方便扩展，增加和删除一个hook都很方便;

### 文件编译




