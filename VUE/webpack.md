webpack:依赖node.js中的npm(node package manager)工具。

模块化开发:在早期，每个人写js代码都会抽取来成为js文件。但是在引入时，可能会造成各个js文件中变量名冲突和覆盖。那早期怎么解决呢？通过闭包，使每个用到的变量在闭包作用域中有效。那么即使变量冲突也造不成影响。但是使用闭包又有其他问题，假如闭包中的变量又想要了呢？这又取不出来了，所以ES5的做法是在闭包中返回一个对象，对象中存了所有的变量。又因为闭包方法会返回这个对象，通过对象可以取到需要的值，而且不用担心变量名覆盖重复等问题。

当然了，那是ES5之前的做法，所以后来才会衍生出模块化开发。

#### 就是将自己写的代码封装成一个模块，然后导出 export ，别人导入 import 需要的变量。(其实原理和原来的ES5差不多)这样就不需要都导入html，而且不会产生冲突。

导入导出，遵循commonJs代码规范。但是浏览器不支持，所以无法识别这种代码。所以要用webpack打包，然后转换成浏览器识别的文件。

使用步骤：

导入导出可以使用两种规范:

一:CommonJS

```
function add(){
  return "5";
}
function dele(){
  return "4";
}

//CommonJs规范导出模块
module.exports ={
  add,
  dele
}
//CommonJs导入
const {add,dele}  =  require('./mathUtil.js')

console.log("相加");
console.log(add());
console.log("相减");
console.log(dele());

```

二:ES6

```

//ES6规范导出
export const age=16
export const name="小敏"

//ES6导入
import {age,name} from "./mathUtil1.js"

console.log("age"+age);
console.log("name"+name);
```

导入导出后因为js并不能识别，需要使用webpack进行打包。

## 命令:webpack  ./src/js/main.js  ./dist/bundle.js 将main入口的js打包成bundle.js

但是这样有个路径问题，这样写路径太复杂了，所以？

避免路径问题：创建配置文件:webpack.config.js（名字固定）

```
const path = require('path');
//path是node中的全局变量，是绝对路径

module.exports = {
    entry: './src/js/main.js',
    output: {
        //__dirname是node全局变量，通过resolve函数就行拼接dist(文件夹名)，合成了需要的路径(默认不能使用相对路径)
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    }
};
```

之后在项目目录直接使用命令:webpack（使用webpack默认使用全局的，因为入口和出口都已经配置了，所以可以直接使用）

但是这又出现一个问题，因为可能从别的地方下载的代码支持webpack3，而我们是webpack4，这就会产生冲突。所以?

## npm:命令npm init （设置为npm可用，会初始化一个package.json文件）

```
{
  "name": "vuetest1", //指定名字
  "version": "1.0.0", //版本
  "description": "\"first use node\"",
  "main": "webpack.config.js", //使用的webpack配置文件
  "scripts": { //可以执行的脚本
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  //通过npm install webpack3.6.0 --save-dev才会出现，表示测试环境下使用的版本
  "devDependencies": {
    "webpack": "^3.6.0"
  }
}
```

## npm run build (会先去找build对应的命令脚本，因为这里配置了局部环境，所以默认使用局部webapck)

所以这个就解决了webpack版本不同的问题。

局部安装(只要是在终端，cmd敲命令，默认使用全局的，npm 的package.json 中的脚本默认先找局部的，找不到再找全局)

npm install webpack3.6.0 --save-dev

> npm install xxx: 安装项目到项目目录下，不会将模块依赖写入devDependencies或dependencies。

> npm install -g xxx: -g的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm cinfig prefix的位置

> npm install -save xxx：-save的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。

> npm install -save-dev xxx：-save-dev的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。

webpack主要是用于打包js，然而像css，图片，ES6转ES5等，都不能使用。这是需要安装*loader* 

 *loader* 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效[模块](https://www.webpackjs.com/concepts/modules)，然后你就可以利用 webpack 的打包能力，对它们进行处理。

## 想要对CSS进行处理需要安装两个loader:

```bash
npm install style-loader --save-dev
npm install --save-dev css-loader
```

在main.js中导入:(CSS本身无需导出)

```js
import style from './file.css'
//导入CSS代码
require('../css/csstest.css')
//两种方式导入
```

在webpack.config.js中进行配置:

```js
    module: {
        rules: [
            {
                test: /\.css$/, //正则表达式，.css结尾文件
                use: [ 'style-loader', 'css-loader' ] //默认是从右往左解析
            }
        ]
    }
```

之后npm run build ：重新生成bundle.js文件