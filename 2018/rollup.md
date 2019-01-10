# 下一代打包工具: rollup.js

## Binding(引用)
ES6 `exports`输出是引用不是具体的值。这个和CommonJS完全不同；

## 编译工具
可以使用下面的工具
* rollup-cli
* grunt-rollup
* fly-rollup

## 命令行
* -v,-version
* -c,-config
* -h,-help
* -w,-watch
* -i,-inpu

## 循环依赖
当使用`AMD`和`Commonjs`时出现循环依赖的情况往往很难解决。但是这对ES6来说很简单；

## ES6 Modules

### 为什么使用ES6 Modules
* 代码组织：ES6 Module允许你将代码分解成合理的chunks，不要把所有代码放在一个文件中；
* 依赖解决：不用在调整`<script>`标签顺序。当需要请求`backbone`不用再告诉浏览器先请求`underscore`
* 多人协作：Modules可以更好处理多人协作产生的冲突问题。
* 代码优化：Modules系统可以更加智能的打包你的代码。

### 杀手锏
`CommonJS`和`AMD`只能有一个`export`。我们经常在很多代码放在一个对象中，例如`underscore` exports 一个 `_`对象，很多方法放在它上面，_.where, _.pluck等；

打包工具很难将代码提取出来，即使你只有使用了一个简单的函数；

ES6的语法可以生成更小的代码。这就是rollup.js诞生的原因。

### ES6-friendly libraries

### Javascript API

#### entry

#### dist

### jsnext:main
设置模块版本；

### 从Esperanto演变过来

### pkg.modules

### plugin

#### 样式文件
* css
* less


### 拓展阅读