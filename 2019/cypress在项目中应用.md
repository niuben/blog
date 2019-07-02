## cypress做单元测试和遇到问题


### 单元测试
`Cypress`集成 `Mocha`、`Chai`等测试库，支持`DDT`和`BDT`两种方式。

当创建一个函数，通过`链式属性`获取对象值，函数内容如下
```js
import _ from "underscore";
export function getValByPath(obj, path){        
    var pathArr = path.split(".");    
    return _.reduce(pathArr, function(prevObj, name){
        return _.isUndefined(prevObj[name]) ? undefined : prevObj[name];
    }, obj);
}

```

创建单元测试，将测试文件引入进来

```js
import {getValByPath} from "./util";    //引入被测试函数
import {moc} from "./moc";              //引入moc数据

describe("This Is Unit Test", function(){
    
    it("Test getValByPath", function(){                
        
        expect(getValByPath(moc, "data.ad")).to.be.a('object'); //判断ad是否是对象

        expect(getValByPath(lotteryData, "data.ad.imgs")).to.be.a('array'); //判断是否是数组;

        expect(getValByPath(lotteryData, "data.ad.price")).to.be.a('number'); //判断是否是数组;

        assert.isOk(getValByPath(lotteryData, "data.ad"));  //判断是否有值;        
    });
});        

```
常见`TDD`命令有 `to`, `be`, `equal` `to`, `be`, `been`, `is`, `that`, `which`, `and`, `has`, `have`, `with`, `at`, `of`, `same`

常见`BDD`命令有 `isExist`, `isNotExist`, `isOk`, `isNotOk`等等 

`BDD`命令很多都是一组两个，表达相反的意思。






### 遇到问题

当使用`ES5 import`方式引用文件时，会出现` 'import' and 'export' may appear only with 'sourceType: module'`错误。原因是`Cypress`对`ES5`支持不好，可以`webpack`构建来解决这个问题.

首先要安装`@cypress/webpack-preprocessor`模块
```js
npm install @cypress/webpack-preprocessor
// yarn add @cypress/webpack-preprocessor
```

下一步 找到`cypress/plugins`文件夹，创建`index.js`, 具体代码如下

```js
const webpack = require('@cypress/webpack-preprocessor');
const options = {
  webpackOptions: require("./webpack.config.js")
} 
module.exports = (on, config) => {  
  on("file:preprocessor", (file)=>{
    if (file.filePath.match(/\.js$/)){
      return webpack(options)(file);
    }
  });  
}
```

下一步 在`index.js`同级目录, 创建`webpack.config.js`, 将js文件通过`babel-loader`处理， 具体代码如下
```js
module.exports = {
    resolve: {
      extensions: ['.js'],
    },
    module: {
      rules: [{
          test: /\.js$/,
          exclude: [/node_modules/],
          use: [{
            loader: require.resolve('babel-loader')
          }]
        }]
    },
  }
```

下一步 配置一下`.babelrc`文件，让`babel`支持`ES5`文件
```js
{
  "presets": ["es2015", "stage-2", "stage-0"],
  "plugins": ["transform-es2015-modules-commonjs"],
  "sourceMaps": true
}

```

从而让`Cypress`支持`ES5`格式



