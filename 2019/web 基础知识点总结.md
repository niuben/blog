## 算法


### 

### 快排

```
function quickSort(arr, index){


}
```


## javascript 50个基础知识点

### javascript 

##### 内存空间
javascript内存分为栈和堆，栈内存中存放一些引用性值和值，堆内存存放一些对象值; 想要访问堆上的对象，需要先从栈内存中取得相应的引用值。  
栈内存执行顺序，符合先进后出的原则;  
栈内存如何实现闭包的？
Javascript是如何执行前端代码的？

#### 调用栈是什么？

#### 事件


#### 数组

splice(start, [length, [item1, [item2, [item3]]]]); 

* start: 表示数组的起始位置
    * 如果start大于数组长度，则在数组尾部添加
    * 如果start小于0，则从数组尾部开始计数，如果start绝对值大于数组长度则0开始

* length: 表示数组的
    * length为0和负数，则在start位置上添加;
    * length大于之后数组的长度，则start之后的数组全部删除
    * length如果没有填写,则start之后的数组都全部删除
* item1: 要添加的值

返回值: 将删除的元素组成一个数组，如果没有删除则返回一个空数组;



#### react-router关于history

* pushstate
* replacestate

```js
window.onpopstate = function(){

}
history.pushState()
histroy.replaceState()
history.go();
history.back();
```
pushState和replaceState不会触发popState事件，只有go和back方法可以;

#### async和await怎么捕获错误?

`async`返回一个promise，则可以在`async`基础上增加一个then方法


#### 数组乱序

sort + random 换位置

随便叫 重新排


#### each、map、reduce

```js
function each(arr, fn){
    for(var i = 0; i < arr.length; i ++){
        fn(arr[i], i);
    }
}
```

```js
function map(arr, fn){
    var mapArray = [];
    for(var i = 0; i < arr.length; i ++){
        mapArray[i] = fn(arr[i], i);
    }
    return mapArray;
}
```

```js
function reduce(arr, fn){
    var sum = arr[0];                
    for(var i = 1; i < arr.length; i ++){
        sum = fn(sum, arr[i]);        
    }
    return sum;
}
```

#### call、apply、bind实现

call(this, a, b);
```js
Function.prototype.call1 = function(context){    
    
    if(typeof context != "object" || context == null ) {
        context = window
    }
    
    context._fn = this;
    /*
    var query = "(";
    for(var i = 1; i < arguments.length; i++){
        query += arguments[i] +  (i != arguments.length - 1 ? "," : "");
    }    
    query += ")";
            
    return eval("context._fn" + query);
    */    

    var argus = [];
    for(var i = 1; i < arguments.length; i++){
        argus.push(arguments[i]);
    }
    argus.join(",");
    return eval("context._fn(" + query + ")");
}
```

apply
```js
Function.prototype.apply1 = function(context){    
    
    if(typeof context != "object" || context == null ) {
        context = window
    }
    
    context._fn = this;    
    
    var query = "(";    
    if(Array.isArray(arguments[1]) || Object.prototype.toString.call(arguments[1]) == "[object array]"){
        for(var i = 0; i < arguments[1].length; i++){
            query += arguments[1][i] +  (i != arguments[1].length - 1 ? "," : "");
        }    
    }
    query += ")";

    var result = eval("context._fn" + query);

    delete context._fn;

    return result;
}
```

bind
```js
Function.prototype.bind1 = function(){
    
    if(typeof context != "object" || context == null ) {
        context = window
    }
    
    context._fn = this;    
    return context._fn;
}
```

#### new实现
1. 执行函数，创建一个实例;
2. 实例继承构造函数原型链;
3. 函数中this都指向实例本身;

```js
    function new(fn, params){

        var obj = {};
        Object.setPrototype(obj,  fn.prototype);
        fn.call(obj, params);
        return obj;
    }
    
```




#### promise

```js
function Promise1(fun){

    this.thenCallback = null;
    this.catchCallback = null;
    this.finallyCallback = null;

    var resolve = function(){
        this.thenCallback && this.thenCallback.apply(this, arguments);
        this.finallyCallback && this.finallyCallback.apply(this, arguments);
    }

    var reject = function(num){
        this.catchCallback && this.catchCallback(this, arguments);
        this.finallyCallback && this.finallyCallback(this, arguments);
    }

    fun(resolve, reject);
    return this;
}

Promise1.prototype.then = function(fun){
    this.thenCallback = fun;
    return this;
}

Promise1.prototype.catch = function(fun){
    this.catchCallback = fun;
    return this;
}

```
#### promise.all、promise.race、promise.resolve、promise.reject


#### ES6继承()
extend、super


#### sleep()

```js
async function sleep(seconds){
    return await new Promise((resolve, reject)=>{
        setTimeout(function(){
            resolve();
        }, seconds * 1000)
    })
}
```


#### require module.exports/import export

`commonjs`规范，`node`里面有个`module`构造器。每个模块都是`module`的实例。`module.exports`是指定`module`暴露内容。`exports`是`module.exports`的引用;

`import`和`export`是`ES6`的模块化规范。`export`可以在多个地方导出;


#### promise、async await、generator

promise的实现
```js

```

async await
```js


```


##### 对new进行模拟

创建一个对象并将构造函数中的this指向这个对象;

##### 
























































##### 事件循环
JS是单进程，任务都是串行执行，所以任务执行慢的时候会出现堵塞情况。因为是单线程的原因，需要设计出一套机制来处理异步操作情况，这套机制就是事件循环。

javascript设计一套任务队列，异步结束后会将异步操作存入队列中，队列具有先到先执行的原则，当进程空闲时会取出队列的第一个任务执行。这就是事件循环。

异步操作主要有四种类型：ajax、setTimeout/setInterval、DOM事件、Promise、i/o
* ajax: 将onready放入任务队列中;
* setTimeout/setInterval: 
* Promise 


只有一个线程和调用栈，所以需要等到调用栈清空后再执行任务队列的值;
异步结束需要进入任务队列等待调用;
调用栈为空时，主线程会去微任务队列查找是否有任务，如果有任务会一次性将所有微任务放入调用栈中，如果没有微任务则取宏任务队列中第一个任务来执行;

有两种任务队列来存放两种任务类型: 微任务和大任务

MacroTask MicroTask;


##### 时间器执行逻辑

setTimeout和setInterval两个时间器运行时，会先在Time xx上注册，当时间到了以后Time XX会把时间器异步任务存到任务队列中。通过事件循环方式执行异步操作。


##### 作用域/作用域链
作用域分为：
* 全局作用域
* 函数作用域
* 动态作用域

作用域链：一种可以向上搜索标识符结构。

动态作用域如下：
```js
var a = 0;
var obj = {
    a: 1,
    test: function(){
        console.log(this.a);
    }
}
var test = obj.test;
test();
```

##### 原型/原型链
原型的定义：构造函授默认具有属性和方法;
构造函数具有原型，原型中包含对象一些属性和方法。原型可以引用其他构造函数的原型从而形成原型链;
`String`、`Object`、`Array`、`Object`、`Boolean`都是原生构造函数，本质是函数，
```js
var a = new String("abcd"); 
typeof a;  //object
var a = "abcd";
typeof a; //string

var b = new Number("1234");
typeof b  //object
var c = new Boolean(true); 
typeof c  //object
```
为什么通过构造函数创建出来和自定义的是两个不同的类型？ 因为构造函数创建是实例，实例必然是一个对象。通过`var`的方式创建的变量值不是一个实例;

##### 构造函数、实例和原型
构造函数: 构造函数是函数的一种。可以通过`new`的方式生成一个实例;
   实例: 可以通过`instanceOf`判断一个变量是否是一个构造函数的实例; 实例也可以使用`constructor`指向它的构造函数;
   原型: 构造函数具有默认属性;

···js
function Test(){

}
var test = new Test(); 

test instanceOf Test // true
test.constructor == Test //true
···
##### 实例和原型
怎么判断一个实例是否属于这个原型？ 


##### 函数继承有哪几种方式？
* 深度拷贝
* 原型链引用
* 原型链实例化

```js
function deepClone(obj){
    var cloneObj = {};
    for(var key in obj){ // 退出条件
        if(typeof obj[key] == "object"){
            cloneObj[key] = deepClone(obj[key]); //更小范围
        }else{
            cloneObj[key] = obj[key]; // 一步操作
        }
    }
    return cloneObj; 
}
```

##### closure

`Closure`可以让执行上下文保留当前的函数上下文，让它不会被回收。从而可以完成很多特性;

`Closure`和作用域链的关系：`Closure`是通过作用域链向上查找的方式，找到对应的变量; 

`Closure`和执行上下文关系：`Closure`包含在执行上下文中;

`执行上下文`: 当程序运行时会生成一个执行上下文环境，称为`Environment Context`, 一般有两种上下文：全局上下文和函数上下文。执行上下文包含两个步骤：创建和执行;

`调用栈`: 程序运行时`javascript`会分配一个栈，栈先将全局上下文推入栈中，当遇到函数时会将函数推入栈中，执行代码时弹出栈头第一个执行上下文然后执行，执行后弹出第二个执行上下文，直到只剩一个全局上下文;


##### 递归
递归函数三个条件
* 退出条件
* 执行逻辑
* 缩小范围


##### 事件捕获和冒泡
事件触发流程是先从根节点到目标节点，再从目标节点回到根节点。前一个步骤是捕获，后一个步骤是冒泡。

##### 垃圾回收
a) 标记性
b) 


##### 函数：匿名函数、构造函数
匿名函数让javascript变得十分灵活，通过匿名函数的组合、传递、返回等方式可以组成各种高阶函数。匿名函数是函数式编程的重要基础。



##### 生成器和迭代器
生成器：可以创建迭代器的
迭代器：每次可以运行到yield处;

```
function * generator(){
    yield 10
}
```

##### Promise
`Promise`可以异步编程，避免过多的回调函数;使用如下
`Promise`怎么用`resolver`和`reject`的判断？直接在实例化过程中进行判断。
`Promise`怎么绑定then和catch？直接绑在`Promise`事例化之后;
```
var fetch = new Promise(function(resolver, reject){
    setTimeout(function(){
        resolver && resolver()
    }, 2000);      
})
```
多个异步操作怎么做？ 可以在`then`和`catch`方法中在`new Promise()`实例化一个`Promise`出来;

`Promise`和`Generate`的区别？ `Promise`底层是通过回调方式实现; `Generate`则使用更加高级和底层的实现方式;

##### 拷贝对象


##### 实现_.map、_.reduce等方法

```js
function map(arr, fun){
    var newArr = [];
    for(var i = 0; i < arr.length; i++){
        newArr[i] = fun.apply(null, [arr[i], i]);
    }
    return newArr;
}

function reduce(value, arr, fun){    
    for(var i = 0; i < arr.length; i++){
        value = fun.apply(null, [value, arr[i]]);
    }
    return value;
}

```
##### 发布订阅模式


##### hook设计模式


##### 算法：排序算法等


##### var做了哪些事情？
变量对象中设计一个属性名，属性值为`undefined`;


##### 浏览器解析流程？


##### Webpack事件流



##### this
`this`是当前作用域指针;

##### 事件e对象


##### 事件流


##### 事件轮询



##### 最优化遍历树


##### 浅/深拷贝
```js
function deepClone(obj){
    var cloneObj = {};
    for(var key in obj){ //退出机制
        cloneObj[key] = typeof obj[key] == "object" ? deepClone(obj[key]) : obj[key];  //缩小范围 
    }
    return cloneObj
}
```

##### call/apply
`call`和`apply`可以调用函数并指定对象，从而可以指定作用域。`call`和`apply`的区别是，`apply`可以通过数组的方式把参数一起传给函数，`call`需要一个一个参数传给函数，对于有多个参数的函数来说使用apply会比较方便;
`call`和`apply`的第一个参数是指定`this`，从而可以指定作用域;

为什么下面的函数的`call`不需要第一个参数？ 也需要第一个参数，只是第一个参数就是值本身，看下面这两个例子:
```js
Array.prototype.concat.call([11], [123]); // [11, 123]'
String.prototype.splice.call("123", ""); //["1", "2", "3"];
```

##### bind
`bind`可以绑定一个函数`this`并且可以传递一些参量，绑定成功后会返回一个函数; 执行返回函数就会在原来作用域下执行函数, `bind`时可以给函数传递参数;


##### caller和callee
`callee`是指向本函数;
`caller`是指想调用本函数的函数;

通过`callee`和`caller`的组合，就可以在不知道本函数名称的情况下，找到调用函数;
``` 
function A(){
    console.log(arguments.calee.caller);
}

function B(){
    A();    
}

B() // function B
```
`caller`和`call`的区别？    `caller`是一个属性，引用指向调用本函数的函数，`call`则是一个方法，可以绑定this和参量;


##### 在数组中删除元素
使用`splice`方法可以实现删除元素，`splice(start, length, value, value....)`，`start`是数组开始位置，`length`是删除的长度，`value`是指插入的值，如果没有`value`的话则只删除.
不过它的副作用是会改变原来数组的值，从而导致变量的不稳定性; 比如下面的代码：
```
var arr = [1, 2, 3];
arr.splice(0, 1); // 1
console.log(arr) //2, 3 
```

##### 模块化设计




##### 执行环境
执行环境叫`call stack`, 目前有几种执行环境：

##### 全局环境

##### 函数环境
当代码执行时，先把全局环境放入`call stack`中，

##### 变量对象
变量对象的三个步骤:
a) 创建变量对象;
b) 检查上下文的函数声明：遇到`function`关键词时，会在变量对象中建立一个属性，属性值是函数的引用值;
c) 检查上下文的变量声明：遇到`var` 变量声明时，会先检查变量对象中是否有同名属性。如果有的话就跳过，如果没有的话设置undefined;

对比下面两段代码的区别
```js
console.log(foo);  // function foo
var foo = 20;
function foo() { console.log('function foo') }
```

```js
console.log(foo);  // function foo
function foo() { console.log('function foo') }
var foo = 20;
```
无论`foo`变量在`function foo`之前还是之后, `console.log(foo)`指向的都是`function foo()`;

##### 上下文环境
创建环境
##### 创建变量对象
##### 确定作用域
##### 确定this

执行环境
##### 变量名赋值
##### 执行函数
##### 

##### 调用栈和作用域链联系和区别

##### map、each


### React

##### 生命周期

* 加载
    * getDafaultProps
    * getDafaultState
    * commponentWillMount
    * commponentDidMount

* 更新
    * commponentWillUpdate
    * commponentDidUpdate
    * commponentWillReceiveProps
    * componentShouldUpdate

* 销毁
    * componentUnMount

##### key
使用`key`是为了可以区别相同的元素，这样DOM节点可以不用重复创建;

##### React diff算法 
React会创建一个`Virtual DOM`保存原来的内容，数据更新后，通过`diff`这棵树来找到更新的地方。具体diff内容：
* 先比较节点类型、属性、属性值、内容
* 当节点不同时，会同时删除这个节点下得所有子节点;

这个算法问题是寻找到一些相同节点时，在节点类型、属性、属性值和内容都相同的情况下，无法定位更新在哪里。所以引入了一个`key`的属性

##### refs
通过`refs`属性可以访问`dom`节点，从而可以操作`dom`节点。使用`refs`属性一定要`componentDidMount`以后;

##### jsx

##### React Hook钩子（BFC)

### 模块化

* export 和 module.export区别
* import和module.export的区别;

* export 和 module.export

*

### CSS

##### line-height: 

##### BFC

##### 布局

##### 单位
px、pt、em、rem

##### flex
* disply: flex
* 盒子属性:
    * flex-direction: row/column(reserver) 定义主轴方向
    * flex-wrap: nowrap/wrap/wrap-reverse 定义元素超出范围的排列方式;
    * flex-flow: flex-direction flex-wrap的简写
    * justify-content: flex-start/flex-end/center/space-between/space-around 定义主轴对齐方式
    * align-items: flex-start/flex-end/center/space-between/space-around/baseline 定义交叉轴对齐方式
    * align-content: flex-start/flex-end/center/space-between/space-around 
    
* 元素属性： 
    
    * order: 
    * flex-grow: <num> 放大的倍数
    * flex-shrink: <num> 缩小的倍数    
    * flex-basis: <num> 设置区域
    * flex: flex-grow/flex-shrink/flex-basic;
    * align-self: flex-start/flex-end/center/space-between/space-around: 单独设置每个元素的对齐方式;
    
em: 是根据字体大小来确定尺寸大小，如果当前字体为12px，那么1em相当于12px，1.5em相当于18px;
rem: root em, 是根据根节点(HTML)字体大小来定的，如果根节点尺寸为12px，那么3em相当于36px;

##### flex布局

##### 字体
默认字体大小： 12px/16px?;
百分比：基于父元素字体大小来计算, 如果父元素16px，子元素设置100%实际尺寸就是16px;

##### max-width/min-width max-height/min-height
场景是区域尺寸不确定时，怎么防止内容的溢出等情况;

##### 选择器
`###### 标签`
`+ > ` 符号

##### 权值
id： 10000
class: 1000
p: 100



## 工具

### webpack 

#### webpack loader为什么是从后往前运行？

#### webpack 思想?




### git rebase


#### git fetch

* fetch-header: 指向已经从远程仓库中拉取的最新版本;
* commit-id: 

`git fetch`拉取最新一个commit-id对应的版本到本地，并更新fetch-head

`git pull = git fetch + git merge`


## 框架

`redux`的思想是将数据集中进行管理; `dispatch`进行调度; `action`传递值;


## 


## 50个基础知识点
css + js + es6 + webapck + react + git + redux 

##### webpack的compiler和compilation
compiler返回webpack的上下午关键
compilation是compiler实例

##### webpack的plugin机制


##### webpack的事件流原理
底层使用`tapable`插件

##### react的hook
useState和useEffect两个方法


##### react的event
自己封装一些属性;

##### react的

##### git add 和 git checkout可以将暂存区文件

##### git merge做了哪些事情？


##### git diff原理

##### css3: box-shadow、transform、translate、key-frame、

##### css3: flex

##### css BFC
BFC(Block Formatting Context) 使用块状元素、浮动元素与其它元素交互的区域; html、浮动和visible
12. css BFC
BFC(Block Formatting Context) 块状格式化上下文 使用块状元素、浮动元素与其它元素交互的区域; html、浮动和visible 
 

### IFC(Inline-Format-Context)行内上下文


### 左右两列，左边固定宽度右边自适应(三种方法)
* 左边: absolute、右边: margin
* 设置左边浮动、右边div自动
* flex/flex-grow

### 

### MVVM
Model 数据
View: 视图
Modal View: 视图数据控制器;

### 双向绑定实现原理


### Proxy对象


### 虚拟DOM实现


### 单页应用路由设计


### 可记忆的斐波那契数列函数


### 防抖和节流


### CSS
css基本上每个公司也都会问，但是问的不会很深，都是一些常见的问题。

盒模型
垂直居中方法
三栏布局
选择器权重计算方式
清除浮动的方法
flex
什么是BFC、可以解决哪些问题
position属性
如何实现一个自适应的正方形
如何用css实现一个三角形

手写题
手写题每个公司都会有，范围也比较固定，如果之前好好准备的话，应该没什么问题。

防抖和节流
深拷贝
数组去重、数组乱序
手写call、apply、bind
继承（ES5/ES6）
sleep函数
实现promise
实现promise.all
实现promise.retry
将一个同步callback包装成promise形式
写一个函数，可以控制最大并发数
jsonp的实现
eventEmitter(emit,on,off,once)
实现instanceof
实现new
实现数组flat、filter等方法
lazyMan
函数currying

ES6
现在基本上都会使用ES6开发。ES6也成为了一个面试必考点。一般面试官都会问用过ES6的哪些新特性，再针对你所回答的进行深入的提问。

let、const、var区别
箭头函数与普通函数的区别
变量的结构赋值
promise、async await、Generator的区别
ES6的继承与ES5相比有什么不同
js模块化（commonjs/AMD/CMD/ES6）

浏览器相关知识
浏览器相关知识几乎是每个公司都会问到的考点，里面涉及的东西也比较多。其中缓存、http2、跨域必问。

从输入URL到呈现页面过程
强缓存、协商缓存、CDN缓存
HTTP2
HTTP状态码
三次握手与四次挥手
跨域（JSONP/CORS）
跨域时如何处理cookie
垃圾回收机制

web安全
一般我都会从xss和csrf说起。

https
什么是xss，如何预防
什么是csrf，如何预防
为什么会造成csrf攻击

事件循环
事件循环绝对是一个必考题。其中涉及到宏任务、微任务、UI渲染等的执行顺序，浏览器端的必须要掌握，node端的有精力的最好也能掌握。
框架（vue）
因为我一直用的都是vue框架，所以问的也都是跟vue相关的。vue中的高频题也不外乎双向绑定、虚拟dom、diff算法这些。

watch与computed的区别
vue生命周期及对应的行为
vue父子组件生命周期执行顺序
组件间通讯方法
如何实现一个指令
vue.nextTick实现原理
diff算法
如何做到的双向绑定
虚拟dom为什么快
如何设计一个组件

webpack
webpack也基本上成了必考的内容，一般会问是否配置过webpack、做过哪些优化之类的。

用过哪些loader和plugin
loader的执行顺序为什么是后写的先执行
webpack配置优化
webpack打包优化（happypack、dll）
plugin与loader的区别
webpack执行的过程
如何编写一个loader、plugin
tree-shaking作用，如何才能生效

作者：阳呀呀
链接：https://juejin.im/post/5d9c2005f265da5bb977c55e
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


### 给定无序整数序列，求其中第K大的数






```js

history.pushState()
history.onpopstate = ()=>{

}

```



##### css haslayout
`haslayout`是每个DOM节点的一个属性，可以通过width|height等方法触发;

##### css 两栏布局

##### css 三栏布局

##### es6 module和 es6 class的区别
`module`提供`export`和`import`;
`es6`是在面向对象方面的升级;

##### asyns、await
async和await是语法糖;
async返回一个promise对象
await返回一个迭代器;


##### yield



拓展：
##### [setTimeout](https://mp.weixin.qq.com/s?__biz=MzI1MTE2NTE1Ng==&mid=2649515867&idx=1&sn=971a3e41da08ddf2da200d9d07af0fb0&chksm=f1efe7d0c6986ec688a746ece15f52c8df78bca37ca2609e75199f5c3fbbabd3fbcc00179885&scene=0&key=564c3e9811aee0abcc036cb111e6e7bdbe3938a8756b5bf3b98a1696b2f16c1e6e3a1b4af159d1ae1dd3e71ee5fae4e0b6655bd9f37cc81efb1174bf3ef39b43f874bc6a0482348422cc5245dfae917f&ascene=0&uin=MzIxNTY1NTU=&devicetype=iMac+MacBookPro11,1+OSX+OSX+10.12.1+build(16B2555)&version=12010210&nettype=WIFI&fontScale=100&pass_ticket=g24dIjS/70EF4QPCYwRMInMa218z6XagvevxLr5Mbzc=) 

##### [javascript 内存空间](https://juejin.im/entry/589c29a9b123db16a3c18adf)

##### 