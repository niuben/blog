---
layout: post
title: env在react打包中起的作用
date: 2013/01/05 19:33
tags: css 
comments: true
---
今天遇到一个问题：本地打包时去除warning，但改成线上打包warning又出现。最后解决办法是每次线上打包都重新打包React文件。为什么本地只要打包一次就可以了，线上要每次都进行打包。

### process.env
`process.env`包含了很多环境信息，比如'user'等

<!-- more -->
### React与process.env关系

> process.env.NODE_ENV !== 'production' ? warning(false, 'Unsupported style property %s. Did you mean %s?%s', name, camelizeStyleName(name), checkRenderMessage(owner)) : void 0;

React每次执行`warning`判断`NODE_ENV`是否是`production`，如果不是的话才会执行`warning`,所以说__想让React不出现`warning`，只要将NODE_ENV_设置为`production`。

### 设置NODE_ENV
使用webpack可以设置`NODE_ENV`


### 设置后react代码是什么？
原始代码
> ```
function warnNoop(publicInstance, callerName) {
      if (process.env.NODE_ENV !== 'production') {
        var constructor = publicInstance.constructor;
          process.env.NODE_ENV !== 'production' ? warning(false, '%s(...): Can only update a mounted or mounting component. ' + 'This usually means you called %s() on an unmounted component. ' + 'This is a no-op. Please check the code for the %s component.', callerName, callerName, constructor && (constructor.displayName || constructor.name) || 'ReactClass') : void 0;
     }
}
```
打包后
>```
function warnNoop(publicInstance, callerName) {
	  if (false) {
	    var constructor = publicInstance.constructor;
	    process.env.NODE_ENV !== 'production' ? warning(false, '%s(...): Can only update a mounted or mounting component. ' + 'This usually means you called %s() on an unmounted component. ' + 'This is a no-op. Please check the code for the %s component.', callerName, callerName, constructor && (constructor.displayName || constructor.name) || 'ReactClass') : void 0;
	  }
	}
```
将`process.env.NODE_ENV !== 'production'`转为`false`，从而不出现warning；
