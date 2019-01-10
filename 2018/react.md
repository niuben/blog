## React介绍

* jsx 
* 事件
* 


### jsx
JSX转换是通过`babel`工具进行的。
JSX的标签将转换为`React.createElement(type, config, children)`
* type: 标签名称;
* config: 属性配置；
* 子元素： 内容或者子element。

#### 不能在JSX中使用`if-else`
`if-else`格式不能在JSX中出现。因为JSX只是转换成对象或者函数的语法糖，看下面基础例子

```
//jsx
<div id={if(true){'ms'}}>hello world</div>
```
转化成react element
```
react.createElement('div', {id: if(true){'ms'}}, "hello world");
```
如果使用`if-else`格式，上面代码格式是不正确的。

#### 使用三元表达式
```
<div id={condition ? 'msg': null}>hello world</div>
```
转化成react element
```
react.createElement('div', {id: condition ? 'msg': null}, "hello world");
```
三元表达式有的时候有点捉襟见肘，可以在JSX之外使用`if-else`来决定使用哪个
```
  var id;
  if(condition) {
    id = msg;
  }
  return(
    <div id={id}>hello world</div>
  );
```
为了更多的内联，可以在jsx中定义`immediately-invoked`函数

```
return (
  <section>
    <h1>Color</h1>
    <h3>Name</h3>
    <p>{this.state.color || "white"}</p>
    <h3>Hex</h3>
    <p>
      {(() => {
        switch (this.state.color) {
          case "red":   return "#FF0000";
          case "green": return "#00FF00";
          case "blue":  return "#0000FF";
          default:      return "#FFFFFF";
        }
      })()}
    </p>
  </section>
);

```

#### Self-Closing标签
在JSX中，只有`<MyComponent />`是正确的，`<MyComponent>`是错误的。

#### JSX扩展属性

#### JSX注意事项

### JS

### 拓展阅读
- [React CreateELement 实现原理](https://github.com/niuben/blog/blob/master/React%20CreateELement%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86.md)
- [React JSX 遍历 obj](https://github.com/niuben/blog/blob/master/React%20JSX%E9%81%8D%E5%8E%86obj.md)
- [react 事件系统](https://github.com/niuben/blog/blob/master/react%20%E4%BA%8B%E4%BB%B6%E7%B3%BB%E7%BB%9F.md)
- [react 非初始化时如何传值](https://github.com/niuben/blog/blob/master/react%20%E9%9D%9E%E5%88%9D%E5%A7%8B%E5%8C%96%E6%97%B6%E5%A6%82%E4%BD%95%E4%BC%A0%E5%80%BC.md)
- [本地 Render React 应用](https://github.com/niuben/blog/blob/master/%E6%9C%AC%E5%9C%B0Render%20React%20%E5%BA%94%E7%94%A8.md)
- [create-react-app 脚手架介绍](https://github.com/niuben/blog/blob/master/create-react-app%E8%84%9A%E6%89%8B%E6%9E%B6%E4%BB%8B%E7%BB%8D.md)
- [env 在 react 打包作用](https://github.com/niuben/blog/blob/master/env%E5%9C%A8react%E6%89%93%E5%8C%85%E4%BD%9C%E7%94%A8.md)