# Babel主要模块介绍和启动逻辑
    * 主要模块分析
        * .babelrc/babel.config.js
        * @Babel/core
        * @Babel/preset-env
        * @Babel/polyfill
        * @Babel/runtime
        * @Babel/helper
        * ****            
    <!-- * Babel启动做了哪些事情？ -->

## 主要模块分析
Babel官方提供一些重要模块，这些模块直接影响Babel打包的流程。我们逐一介绍一下。


#### .babelrc/babel.config.js
.babelrc和babel.config.js都是Babel的配置文件。它们的区别是：
* babel.config.js是一个项目的配置文件，.babelrc是一个文件配置文件;


#### @Babel/core
@Babel/core提供Babel核心代码，包括Babel这个关键字。


#### @Babel/preset-env
@Babel/preset-env提供最新一些插件和polyfill，从而降低用户配置的成本。preset-env模块是有官方进行维护更新，会及时响应EMCAScript版本的更新。

@Babel/preset-env整体代码结构如下:
```js    
    module.exports = function(){
        return {
            plugins: [
                "PluginA",
                "PluginB",
                "PluginC",
            ]
        }
    }
```

#### @Babel/polyfill
Babel只对一些语法格式做转移，不包括对新函数功能实现, 比如generate/yield功能等等。
polyfill则可以实现新的函数功能，实现逻辑是用特定函数进行替换。


#### @Babel/runtime
@Babel/runtime在代码运行调用数据类型、方法。这样做一个好处是动态生成方法，从而可以调用公共方法，从而解决编译时重复代码的问题。


#### @Babel/helper
@Babel/helper提供一些公共方法。比如对一些函数的解析，其他模块都可以直接用这个方法。

#### @Babel/register
注册`require`方法；

#### @Babel/type
判断节点类型库

#### @Babel/cli

#### @Babel/generator
生成代码

#### @Babel/parser
编译代码

<!-- ## Babel启动做了哪些事情？

#### Babel是如何加载配置文件？

1. Babel启动时加载.babelrc配置文件, 获取presets、plugins等字段内容;

#### 插件如何执行的？ -->





