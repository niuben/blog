## javascript 50个基础知识点

### javascript 
1. 内存空间
javascript内存分为栈和堆，栈内存中存放一些引用性值和值，堆内存存放一些对象值; 想要访问堆上的对象，需要先从栈内存中取得相应的引用值。  
栈内存执行顺序，符合先进后出的原则;  
栈内存如何实现闭包的？
Javascript是如何执行前端代码的？

2. 事件循环
JS是单进程，任务都是串行执行，所以任务执行慢的时候会出现堵塞情况。因为是单线程的原因，需要设计出一套机制来处理异步操作情况，这套机制就是事件循环。

javascript设计一套任务队列，异步结束后会将异步操作存入队列中，队列具有先到先执行的原则，当进程空闲时会取出队列的第一个任务执行。这就是事件循环。

3. 时间器执行逻辑

setTimeout和setInterval两个时间器运行时，会先在Time xx上注册，当时间到了以后Time XX会把时间器异步任务存到任务队列中。通过事件循环方式执行异步操作。


4. 作用域/作用域链
作用域分为：
* 全局作用域
* 函数作用域
* 动态作用域

动态作用域如下：
```js
var  a = 10;
var obj = {
    a: 1,
    test: function(){
        console.log(obj.a);
    }
}
var test = obj.test;
```

5. 原型/原型链
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

6. 构造函数、实例和原型
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


7. 函数继承有哪几种方式？
* 深度拷贝
* 原型链引用
* 原型链实例化


8. closure

`Closure`可以让执行上下文保留当前的函数上下文，让它不会被回收。从而可以完成很多特性

`Closure`和作用域链的关系：`Closure`是通过作用域链向上查找的方式，找到对应的变量。`Closure`

`Closure`和执行上下文关系：`Closure`包含在执行上下文中;

`执行上下文`: 当程序运行时会生成一个执行上下文环境，称为`Environment Context`, 一般有两种上下文：全局上下文和函数上下文。执行上下文包含两个步骤：创建和执行。

`调用栈`: 程序运行时`javascript`会分配一个栈，栈先将全局上下文推入栈中，当遇到函数时会将函数推入栈中，执行代码时弹出栈头第一个执行上下文然后执行，执行后弹出第二个执行上下文，直到只剩一个全局上下文。


9. 递归
递归函数三个条件
* 退出条件
* 执行逻辑
* 缩小范围


10. 事件捕获和冒泡
事件触发流程是先从根节点到目标节点，再从目标节点回到根节点。前一个步骤是捕获，后一个步骤是冒泡。

11. 垃圾回收


12. 函数：匿名函数、构造函数
匿名函数让javascript变得十分灵活，通过匿名函数的组合、传递、返回等方式可以组成各种高阶函数。匿名函数是函数式编程的重要基础。



15. 生成器和迭代器
生成器：可以创建迭代器的
迭代器：每次可以运行到yield处;

```
function * generator(){
    yield 10
}

```

16. Promise
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

17. 拷贝对象


18. 实现_.map、_.reduce等方法

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
19. 发布订阅模式


20. hook设计模式


21. 算法：排序算法等


22. var做了哪些事情？
变量对象中设计一个属性名，属性值为`undefined`;


23. 浏览器解析流程？


25. Webpack事件流


27. this
`this`是当前作用域指针;

28. 事件e对象


29. 事件流


30. 最优化遍历树


31. 浅/深拷贝
```js
function deepClone(obj){
    var cloneObj = {};
    for(var key in obj){ //退出机制
        cloneObj[key] = typeof obj[key] == "object" ? deepClone(obj[key]) : obj[key];  //缩小范围 
    }
    return cloneObj
}
```

32. call/apply
`call`和`apply`可以调用函数并指定对象，从而可以指定作用域。`call`和`apply`的区别是，`apply`可以通过数组的方式把参数一起传给函数，`call`需要一个一个参数传给函数，对于有多个参数的函数来说使用apply会比较方便;
`call`和`apply`的第一个参数是指定`this`，从而可以指定作用域;

为什么下面的函数的`call`不需要第一个参数？ 也需要第一个参数，只是第一个参数就是值本身，看下面这两个例子:
```js
Array.prototype.concat.call([11], [123]); // [11, 123]'
String.prototype.splice.call("123", ""); //["1", "2", "3"];
```

33. bind
`bind`可以绑定一个函数`this`并且可以传递一些参量，绑定成功后会返回一个函数; 执行返回函数就会在原来作用域下执行函数, `bind`时可以给函数传递参数;

33. caller和callee
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


34. 在数组中删除元素
使用`splice`方法可以实现删除元素，`splice(start, length, value, value....)`，`start`是数组开始位置，`length`是删除的长度，`value`是指插入的值，如果没有`value`的话则只删除.
不过它的副作用是会改变原来数组的值，从而导致变量的不稳定性; 比如下面的代码：
```
var arr = [1, 2, 3];
arr.splice(0, 1); // 1
console.log(arr) //2, 3 
```

33. 模块化设计
C



34. 执行环境
执行环境叫`call stack`, 目前有几种执行环境：
1. 全局环境
2. 函数环境
当代码执行时，先把全局环境放入`call stack`中，


35. 变量对象
变量对象的三个步骤:
a) 创建变量对象
b) 检查上下文的函数声明：遇到`function`关键词时，会在变量对象中建立一个属性，属性值是函数的引用值；
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

36. 上下文环境
创建环境
1. 创建变量对象
2. 确定作用域
3. 确定this

执行环境
1. 变量名赋值
2. 执行函数
3. 


37. 调用栈和作用域链联系和区别

### React

1. 生命周期

2. key
使用`key`是为了可以区别相同的元素，这样DOM节点可以不用重复创建;

3. React diff算法 
React会创建一个`Virtual DOM`保存原来的内容，数据更新后，通过`diff`这棵树来找到更新的地方。具体diff内容：
* 先比较节点类型、属性、属性值、内容
* 当节点不同时，会同时删除这个节点下得所有子节点;

这个算法问题是寻找到一些相同节点时，在节点类型、属性、属性值和内容都相同的情况下，无法定位更新在哪里。所以引入了一个`key`的属性

4. refs
通过`refs`属性可以访问`dom`节点，从而可以操作`dom`节点。使用`refs`属性一定要`componentDidMount`以后;

5. jsx



### CSS

1. line-height

2. BFC

3. 布局

4. 单位
px、pt、em、rem

5. flex
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
    






### 算法






拓展：
    1. [setTimeout](https://mp.weixin.qq.com/s?__biz=MzI1MTE2NTE1Ng==&mid=2649515867&idx=1&sn=971a3e41da08ddf2da200d9d07af0fb0&chksm=f1efe7d0c6986ec688a746ece15f52c8df78bca37ca2609e75199f5c3fbbabd3fbcc00179885&scene=0&key=564c3e9811aee0abcc036cb111e6e7bdbe3938a8756b5bf3b98a1696b2f16c1e6e3a1b4af159d1ae1dd3e71ee5fae4e0b6655bd9f37cc81efb1174bf3ef39b43f874bc6a0482348422cc5245dfae917f&ascene=0&uin=MzIxNTY1NTU=&devicetype=iMac+MacBookPro11,1+OSX+OSX+10.12.1+build(16B2555)&version=12010210&nettype=WIFI&fontScale=100&pass_ticket=g24dIjS/70EF4QPCYwRMInMa218z6XagvevxLr5Mbzc=) 

    2. [javascript 内存空间](https://juejin.im/entry/589c29a9b123db16a3c18adf)

    3. 