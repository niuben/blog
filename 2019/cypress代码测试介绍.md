## cypress代码测试介绍

目录
* 要解决什么问题？
* cypress是什么？    
* cypress怎么使用？    
* cypresss不足    

### 要解决的问题

__前端代码越来越复杂，测试成本越来越高.__

常见问题场景：

1. 新创建的函数没有单独测试入口，需要直接放在业务代码中测试;
2. 新创建的函数需要多步操作才能调用，每次调试需要操作多次才能调用我们想要的函数;
3. 新创建的函数传入一些数据成功，换一批数据就报错了;


### cypress是什么？
cypress是前端测试工具。具有单元测试、集合测试和端到端测试整体功能，实现对代码和UI一站式测试需求。
![cypress_compare](../static/cypress_compare.jpg)


### cypress怎么使用? 

···js
$ yarn add cypress --dev
···

会在项目中增加`cypress`文件夹, 先将测试代码放在`integration`中;

下一步 在`integration`文件夹中，创建add.func.spec.js文件，并增加单元测试代码:

单元测试
```js
function add(x, y){
    return x + y;
}

describe("Test Add Function", ()=>{    
    it("test 1 + 1 equal 2 ?", ()=>{                
        expect(add(1, 1)).to.equal(2);
    })
});

```

下一步 open `Cypress`
```
yarn run cypress open
```

打开下面弹出框  
![cypress_run_sheenshot](../static/cypress_run_sheenshot.png)

下一步 点击`add.func.spec.js`文件, 会打开浏览器  
![cypress_browser](../static/cypress_browser.png)

最终显示代码测试通过;


#### 常见的断言语法
* equal()、not.equal()： 相等和不相等
* above(): 大于
* true、undefined、null、NaN: 类型判断

UI测试
```js
context('Waiting', () => {
  beforeEach(() => {
    cy.visit('https://example.cypress.io/commands/waiting')
  })
  // BE CAREFUL of adding unnecessary wait times.
  // https://on.cypress.io/best-practices#Unnecessary-Waiting

  // https://on.cypress.io/wait
  it('cy.wait() - wait for a specific amount of time', () => {
    cy.get('.wait-input1').type('Wait 1000ms after typing')
    cy.wait(1000)
    cy.get('.wait-input2').type('Wait 1000ms after typing')
    cy.wait(1000)
    cy.get('.wait-input3').type('Wait 1000ms after typing')
    cy.wait(1000)
  })
})
```
![cypress_browser](../static/cypress_wait.png)

#### cypress有什么不足
cypress在跨浏览器兼容测试方面有些不足，需要配合其他工具一起使用。
![cypress_browser](../static/cypress_compare.png)