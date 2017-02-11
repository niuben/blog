## webpack2新功能之tree-sharking 
webpack2正式发布，这个大版本历时一年多时间开发终于发布稳定版本。
tree-sharking是这个版本一大亮点。tree-sharking这个概念是rollup.js提出来。tree-sharking的功能：删除exports中没有使用的代码。这是基于ES6模块化一个优化功能，所以前提是你的代码是用ES6写的。

举个例子，下面是math模块
```js
// This function isn't used anywhere
export function square(x) {
    return x * x;
}

// This function gets included
export function cube(x) {
    return x * x * x;
}
```
main.js入口文件引用

```js
import {cube} from './maths.js';
console.log(cube(5)); // 125
```






