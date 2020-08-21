## 构建自己的React —— Fiber

本文实现4个功能

1. CreateElement
2. Render
3. Concurrent Mode
4. Fibers

### CreateElement

1. 存储Element数据对象;
2. 创建字节点

一个简单模板:
```js
var element = <h1 title="foo">Hello</h1>
```
通过`Babel`转换后
```js
const element = React.createElement(
  "h1",
  { title: "foo" },
  "Hello"
)
```

有嵌套关系的模板:
```js
const element = (
  <div id="foo">
    <a>bar</a>
    <b />
  </div>
)
```
通过`Babel`转换后
```js
const element = React.createElement(
  "div",
  { id: "foo" },
  React.createElement("a", null, "bar"),
  React.createElement("b")
)
```

createElement源码实现;
```js
function createElement(type, props, ...children) {
  return {
    type: type,
    props: {
      ...props,
      children: children.map((child) => {
        return typeof child == "object" ?
          child :
          createTextElement(child);
      })
    }
  }
}

function createTextElement(text) {
  return {
    type: "TEXT_ELEMENT",
    props: {
      nodeValue: text,
      children: [],
    }
  }

```

### render

render函数主要功能是创建DOM

1. 创建DOM节点;
2. 设置节点属性;
3. 创建DOM子节点;

创建DOM节点
```js
function render(element, container) {
  const dom = document.createElement(element.type);
​
  container.appendChild(dom)
}
```

判断DOM类型
```js
function render(element, container) {
    const dom =
        element.type == "TEXT_ELEMENT"
        ? document.createTextNode("")
        : document.createElement(element.type)
    ​
    container.appendChild(dom)
}    
```

处理属性
```js
function render(element, container) {
    const dom =
        element.type == "TEXT_ELEMENT"
        ? document.createTextNode("")
        : document.createElement(element.type)

    const isProperty = key => key !== "children"
    Object.keys(element.props)
        .filter(isProperty)
        .forEach(name => {
            dom[name] = element.props[name]
        })
    ​element.props.children.forEach(child =>
        render(child, dom)
    )
    ​
    container.appendChild(dom)
}    
```

### Concurrent Mode（并发模式)

1. requestIdleCallback API
2. nextUnitOfWork


`requestIdleCallback`可以在浏览器空闲时运行制定函数;

```js
let nextUnitOfWork = null
​
function workLoop(deadline) {
  let shouldYield = false
  while (nextUnitOfWork && !shouldYield) {
    nextUnitOfWork = performUnitOfWork(
      nextUnitOfWork
    )
    shouldYield = deadline.timeRemaining() < 1
  }
  requestIdleCallback(workLoop)
}
​
function performUnitOfWork(nextUnitOfWork) {
  // TODO
}

requestIdleCallback(workLoop)
```


### Fibers

1. Fibers解决什么问题？
2. 如何将Tree分解成Fiber？
3. 每个Fiber如何被执行？
4. Fiber之间如何建立联系？
5. 一个Fiber如何创建DOM？
6. render的变化?
6. 整个Fibers运行过程是什么？


React16版本之前是通过递归方式进行`render`。递归有个缺点是中断必须执行完毕，从而当`tree`节点很多、层级很深时，`render`执行时间会很长。  


Fiber Tree结构图  
![](https://pomb.us/static/19c304dcb3824b14722691ded539ecdb/ac667/fiber4.png);

单个Fiber Tree的数据结构
```js
const h1Fiber = {
    type: element.type,
    props: element.props,
    parent: fiber,
    dom: null,
    child: childFiber,
    siblingChild: sibling
}
​
```
将树状结构变成链表结构。链表结构可以拆分处理，因为通过链表任意一个节点都可以找其他所有节点。

通过`performUnitOfWork`执行每个`Fiber`
```js
function performUnitOfWork(fiber) {
  if (!fiber.dom) {
    fiber.dom = createDom(fiber)
  }

  if (fiber.parent) {
    fiber.parent.dom.appendChild(fiber.dom)
  }

  //上面Fiber
}
```


通过循环DOM节点建立依赖关系？
```js
const elements = fiber.props.children
let index = 0
let prevSibling = null

 while (index < elements.length) {
    const element = elements[index]
​
    const newFiber = {
      type: element.type,
      props: element.props,
      parent: fiber,
      dom: null,
    }
​
    if (index === 0) {
      fiber.child = newFiber
    } else {
      prevSibling.sibling = newFiber
    }
​
    prevSibling = newFiber;
    index++;
  }

  if (fiber.child) {
    return fiber.child
  }
  let nextFiber = fiber
  while (nextFiber) {
    if (nextFiber.sibling) {
      return nextFiber.sibling
    }
    nextFiber = nextFiber.parent
  }

```

给每个Fiber创建DOM节点
```js
function createDom(fiber) {
  const dom =
    fiber.type == "TEXT_ELEMENT"
      ? document.createTextNode("")
      : document.createElement(fiber.type)
​
  const isProperty = key => key !== "children"
  Object.keys(fiber.props)
    .filter(isProperty)
    .forEach(name => {
      dom[name] = fiber.props[name]
    })
​
  return dom;
}
```

通过`requestIdelCallback`进行将所有`Fiber`链接起来;

render的变化
```js
function render(element, container) {
  nextUnitOfWork = {
    dom: container,
    props: {
      children: [element],
    },
  }
}
```
通过上面几个步骤实现完整的Fiber Tree;