## 问题
一个React多页面应用需要做首屏渲染，但是服务器环境是PHP环境，无法使用react-dom-server方法。调研后确定了两个技术方案
* 后端方案：使用V8JS拓展渲染React代码，并将生成DOM放入到HTML中；
* 前端方案：使用react-snapshot模块，通过前端编译方式将React代码放入到HTML中；

## react-snapshot方案
### 原理
* 创建一个Node服务器渲染页面；
* 通过jsdom模块获取页面中代码；
* 给源文件加上渲染后代码；
react-snapshot模拟出React执行后的结果，然后将结果放入源代码中。

### 多页面支持
snapshot源代码中是不支持多页面的，所有的页面都被映射到`200.html`中
