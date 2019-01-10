## Inquirer.js
一个可交互的命令行模块。

### 安装
`npm install inquirer`

### 方法

#### inquirer.prompt(questions) -> promise
启动一个确认的交互页面 

* questions: 包含一个Quesetion对象
* 返回一个Promise

#### inquirer.registerPrompt(name, prompt)
在name下注册一个Prompt组件


### Question对象
* type:(String) 确认的类型。默认是：input 其他的值：input，confirm，list，rawlist, expand, checkbox, password, editor;
* name:(String) 通过这个name获取用户选择或者输入的值；
* message:(String|Function) 问题是什么。如果是一个函数，__第一个参数会获取到已经选择过答案信息。__
* default:(String|Number|Array|Function) 默认值。当用户没有选择或者输入这个值别使用。如果定义为一个函数时返回一个默认值，并且第一个参数会获取到已经选择过答案信息。
* choices:(Array|Function) 数组或者函数都会返回一个选项数组. 如果定义为一个函数时返回一个默认值，并且第一个参数会获取到已经选择过答案信息。选项数组可以是字符串也可以是对象。如果是对象包含三个属性：name,value,short. name是显示内容，value是保存起来的值，short是选择后显示的值；
* validate:(Function) 接受用户输入和返回一个true。如果值非法应该返回一个错误信息，否则如果只返回一个false,则显示一个默认错误信息；
* filter:(Function) 接受用户输入并且返回值会添加到已选择对象中；
* when:(Function,Boolean) 
* pageSize:(Number) 修改行数

### 代码示例
```js
'use strict';
var inquirer = require('inquirer');

inquirer.prompt([
  {
    type: 'list',
    name: 'theme',
    message: function(a, b){
    	console.log(a);
    	console.log(b);
    	return 'What do you want to do?';
    },
    choices: [
      'Order a pizza',
      'Make a reservation',
      new inquirer.Separator(),
      'Ask for opening hours',
      {
        name: 'Contact support',
        disabled: 'Unavailable at this time'
      },
      'Talk to the receptionist'
    ]
  },
  {
    type: 'list',
    name: 'size',
    message: function(a, b){
    	console.log(a);
    	return 'What size do you need?';
    },
    choices: ['Jumbo', 'Large', 'Standard', 'Medium', 'Small', 'Micro'],
    filter: function (val) {
      return val.toLowerCase();
    }
  }
]).then(function (answers) {
  console.log(JSON.stringify(answers, null, '  '));
});
```











