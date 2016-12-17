---
layout: post
title: Webpack CommonChunkPlugin详解
date: 2016/7/13 20:46:25
tags: webpack 
comments: true
---
## webpack介绍

Webpack最近比较流行前端构建工具.它通过分析代码中require调用方式，将代码进行模块化打包，相关多个JS文件会打包成一个文件。同时Webpack还具有MD5版本、内置开发调试服务器等功能；相比Grunt和Gulp等任务流构建工具，Webpack具有模块化打包能力。

CommonChunkPlugin插件是Webpack官方插件，用于公共文件提取打包；将多个模块都调用到文件，打包到一个公共Chunk中；避免多个模块重复打包相同文件，大大降低整体文件大小；


## CommonChunkPlugin参数列表

* name: 公共包名称叫什么。如果存在包名和name相同的话，这个包会变成公共包；如果name是数组会调用数组中每个名字；每个名字都可能是公共文件；如果这个参数没有赋值，所有包都会被选中；
* filename: 公共包文件格式；
* minChunks: 一个包放入公共包需要的最小次数；
* chunks: 那些包会被选中，如果这个值忽略，所有entry包都会被选中；
* children: 如果为true，公共包子元素都会被选中；
* async: 如果为true，会创建一个async公共包，会和其他包共同加载；
* minsize: 可以打包的最小质量；比如100KB；

## CommonChunkPlugin实现原理

#### 调试Webpack

前端需要使用工具来调试，方便知道程序中变量的值和逻辑运行情况；node-inspector是专门调试node程序；使用之前需要将node版本升级，如果版本过低，有些功能无法使用；按照使用如下：

* npm install node-inspector -g
* npm-inspector
* 进入项目文件夹，找到webpack.config.js文件；
* node --debug-brk node_modules/webpack/bin/webpack.js(webpack所在文件夹）
* 打开chrome浏览器，输入http://127.0.0.1:8080/?ws=127.0.0.1:8080&port=5858；

<!-- more -->

#### 公共文件数据结构

有三个数据类型需要理解：block、module、chunk；

* block：
* module：一个commonJS或者AMD文件；
* chunk：多个module打包生成的；

#### 实现原理

```js
if(!chunkNames && (selectedChunks === false || async)) {
   commonChunks = chunks;
}else if(Array.isArray(chunkNames)) {
    commonChunks = chunkNames.map(function(chunkName) {
    return chunks.filter(function(chunk) {
        return chunk.name === chunkName;
    })[0];
 });
} else {
    commonChunks = chunks.filter(function(chunk) {
        return chunk.name === chunkNames;
    });
}
```

#### 通过配置中name属性，将commonChunks进行赋值；
```js
if(Array.isArray(selectedChunks)) {
    usedChunks = chunks.filter(function(chunk) {
        if(chunk === commonChunk) return false;
        return selectedChunks.indexOf(chunk.name) >= 0;
    });
} else if(selectedChunks === false || async) {
    usedChunks = (commonChunk.chunks || []).filter(function(chunk) {           
       return async || chunk.parents.length === 1;
    });
} else {
    if(!commonChunk.entry) {
        compilation.errors.push(new Error("CommonsChunkPlugin: While running in normal mode it's not allowed to use a non-entry chunk (" + commonChunk.name + ")"));
        return;
    }
    usedChunks = chunks.filter(function(chunk) {
        var found = commonChunks.indexOf(chunk);
        if(found >= idx) return false;
        return chunk.entry;
    });
}
```

#### 获得被使用的包
```js
usedChunks.forEach(function(chunk) {
    chunk.modules.forEach(function(module) {
        var idx = commonModules.indexOf(module);
        if(idx 小于 0){
            commonModules.push(module);
            commonModulesCount.push(1);
        }else {
            commonModulesCount[idx]++;
        }
    });
})
```

#### 使用包中每个模块被调用的个数；
```js
commonModulesCount.forEach(function(count, idx) {
    var module = commonModules[idx];
    if(typeof minChunks === "function") {
        if(!minChunks(module, count))
        return;
    } else if(count 小于 (minChunks || Math.max(2, usedChunks.length))) {
        return;
    }
    reallyUsedModules.push(module);
})
```

#### 将大于minChunks的module存入到reallyUsedModules中；
```js
reallyUsedModules.forEach(function(module) {
    usedChunks.forEach(function(chunk) {
        if(module.removeChunk(chunk)) {
            if(reallyUsedChunks.indexOf(chunk) 小于 0)
               reallyUsedChunks.push(chunk);
            }
        });
        commonChunk.addModule(module);
        module.addChunk(commonChunk);
   });
});
```

#### 将多次调佣module存入CommonChunk中；
```js
reallyUsedChunks.forEach(function(chunk) {
    if(chunk.initial || chunk.entry)
        return;
    chunk.blocks.forEach(function(block) {
        block.chunks.unshift(commonChunk);
        commonChunk.addBlock(block);
    });
});
asyncChunk.origins = reallyUsedChunks.map(function(chunk) {
    return chunk.origins.map(function(origin) {
        var newOrigin = Object.create(origin);
        newOrigin.reasons = (origin.reasons || []).slice();
        newOrigin.reasons.push("async commons");
        return newOrigin;
    });
}).reduce(function(arr, a) {
    arr.push.apply(arr, a);
    return arr;
}, []);
```

end
