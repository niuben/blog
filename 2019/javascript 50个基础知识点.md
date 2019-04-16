javascript 50个基础知识点

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
var obj = {
    a: 1,
    test: function(){
        console.log(obj.a);
    }
}
var test = obj.test;
```

5. 原型/原型链
构造函数具有原型，原型中包含对象一些属性和方法。原型可以引用其他构造函数的原型从而形成原型链。

6. 实例和原型


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



13. 数据类型和转换


14. 内存溢出几种情况


15. 生成器和迭代器
生成器：可以创建迭代器的
迭代器：每次可以运行到yield处;

```
function * generator(){
    yield 10
}

```

16. promise



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


24. React diff算法


25. Webpack事件流


26. 事件流


27. this


28. 事件e对象


29. 事件流


30. 最优化遍历树


31. 浅/深拷贝


32. call/apply


33. 模块化设计


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


























拓展：
    1. [setTimeout](https://mp.weixin.qq.com/s?__biz=MzI1MTE2NTE1Ng==&mid=2649515867&idx=1&sn=971a3e41da08ddf2da200d9d07af0fb0&chksm=f1efe7d0c6986ec688a746ece15f52c8df78bca37ca2609e75199f5c3fbbabd3fbcc00179885&scene=0&key=564c3e9811aee0abcc036cb111e6e7bdbe3938a8756b5bf3b98a1696b2f16c1e6e3a1b4af159d1ae1dd3e71ee5fae4e0b6655bd9f37cc81efb1174bf3ef39b43f874bc6a0482348422cc5245dfae917f&ascene=0&uin=MzIxNTY1NTU=&devicetype=iMac+MacBookPro11,1+OSX+OSX+10.12.1+build(16B2555)&version=12010210&nettype=WIFI&fontScale=100&pass_ticket=g24dIjS/70EF4QPCYwRMInMa218z6XagvevxLr5Mbzc=) 

    2. [javascript 内存空间](https://juejin.im/entry/589c29a9b123db16a3c18adf)

    3. 