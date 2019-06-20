## cypress代码测试介绍

目录
* 要解决什么问题？
* cypress是什么？    
    * 断言库
* cypress具有哪些功能？
    * screenshot
    * 视频
    * 任务栏介绍    
* cypress怎么使用？
    * 命令行启动
    * 
* cypress在实际项目中的应用?
    * 编写单元测试
    * 进行UI检测

* cypresss不足
    *

### 要解决的问题

__前端代码越来越复杂，测试成本越来越高.__

常见问题场景：

1. 新创建的函数没有单独测试入口，需要直接放在业务代码中测试;
2. 新创建的函数需要多步操作才能调用，每次调试需要操作多次才能调用我们想要的函数;
3. 新创建的函数传入一些数据成功，换一批数据就报错了;


### cypress是什么？
cypress具有




### cypress怎么使用

···
$ yarn add cypress --dev
···

下一步 open `Cypress`

```
yarn run cypress open 
```

增加测试代码
```js
describe('login', () => {
  beforeEach(() => {
    visitLoginPage()
  })

  it('A User logs in and sees a welcome message', () => {
    loginWith('michael@example.com', 'passsword')

    expect(cy.contains('Welcome back Michael')).to.exist
  })

  it('A User logs off and sees a goodbye message', () => {
    loginWith('michael@example.com', 'password')
    logout()

    expect(cy.contains('Goodbye! See you soon!'))
  })
})

const visitLoginPage = () => {
  cy.visit('http://localhost:3000')
}

const loginWith = (email, password) => {
  cy.get('[name="email"]').type(email)
  cy.get('[name="password"]').type(password)
  cy.get('button').click()
}
const logout = () => {
  cy.get('button').click()
}
```

还可以使用下面命令：
```js
yarn run cypress run 
```



