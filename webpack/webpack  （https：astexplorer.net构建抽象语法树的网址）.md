## 浏览器端模块化

#### 浏览器的模块化 --- es6module 的问题：
> ##### 1.效率问题：模块惊喜导致引入了过多的js文件，带来了过多的请求，降低了页面的访问效率

> ##### 2.兼容性问题：只能使用es6 modules

> ##### 3.工具问题：浏览器不支持npm下载的第三方包

#### 为什么在node端支持npm等一系列上述问题？
> ##### 因为浏览器请求页面需要请求并下载资源到用户的浏览器上。而node可以直接读取本地文件`fs.readFile()`

#### 根本原因： 在浏览器端，开发时态（devtime）和运行时态（runtime）的侧重点不一样
> ##### 开发时态：
>> 1.模块划分越细致越好
>> 2.支持多种模块化标准
>> 3.支持npm和其他包下载管理工具
>> 4.能够解决其他工程化问题

#### 根本原因： 在浏览器端，开发时态（devtime）和运行时态（runtime）的侧重点不一样
> ##### 运行时态：
>> 1.文件越少越好
>> 2.文件体积越小越好
>> 3.代码内容越乱越好
>> 4.所有浏览器都要兼容
>> 5.能够解决运行时的问题，主要是执行的效率问题

### 常用的构建工具：`webpack` `gulp` `grunt` `fis` `browserify`

## 安装并使用

#### 安装：`npm install -D webpack webpack-cli`
> ##### `weboack-cli`提供`webpack`命令来供我们在命令行运行webpack

#### 使用：
> ##### `在命令行输入npx webpack`，默认会以`./src/index.js`为入口，`./dist/main.js`为出口进行打包

> ##### `npx webpack --mode=production/development`打包并设置打包后的文件是生产环境还是开发环境，生产环境会进行压缩等一系列操作，生成更有利于部署到服务器上面的代码

> ##### `npx webpack --watch` 监听`./src/index.js`文件的变化，如果变化了就自动打包

## 模块化兼容

#### 用commonJS导入或者导出，为什么用es6 module依然可以导入或者导出，两者为什么可以配合使用？
> ##### es6 export -- commonJs require()： 
>> ```
>> //a.js
>> export const a = 'a'
>> export const b = 'b'
>> export default 'c'
>> //b.js
>> const obj = require('./a.js');/*相当于--->*/  import * as obj from './a.js'
>> //obj {a:'a',b:'b',default:'c'}
>> ```

> ##### commonJS export -- es6 import： 
>> ```
>> module.exports = {
>>  a:'a',
>>  b:'b',
>>}
>>
>>import * as obj from './a.js' /*或者*/ import obj from './a' /*相当于--->*/ const obj = require('./a.js')  /*也可以用结构import {a,b,c} from ''*/
>>//obj {a:'a',b:'b',c:'c'}
>>
>> ```



## webpack配置文件

#### 默认将`webpack.config.js`文件作为配置文件，但也可以通过CLI参数`--config`来指定某个配置文件

#### 配置文件中通过CommonJS模块导出一个对象


```
//webpack.config.js 

module.exports = {
    mode: 'production/development' /*编译模式*/,
    entry: '' /*入口 (默认入口为./src/index.js)*/, 
    output: {} /*出口*/,
}
```

## devtool配置 --- 解决调试问题（为了能在源码中显示错误信息）

#### source map 源码地图 （与webpack无关）

#### 使用：
> ##### 最好在开发环境中使用，因为源码地图的`.map`文件过大不利于网页流畅度，也更容易让其他人盗取源码

#### webpack中使用source map

## webpack编译过程

#### 分为三个部分
> ##### 1.初始化
>> **默认配置** **CLI参数** **配置文件**三个配置的融合是通过`yargs`这个第三方库来完成的

> ##### 2.编译
>> ①创建chunk：chunk是webpack在内部构建过程中的一个概念，译为`块`，它表示通过某一个入口找到的所有依赖的统称，（默认根据`./src/index.js`来创建chunk）（多个入口对应多个chunk，它们是一一对应的关系）     
>> ②构建所有依赖模块   
>> ③产生 chunk assets     
>> ④合并 chunk assets   

> ##### 3.输出


## loader

#### 本质就是一个函数传进来一个的字符串，然后返回一个字符串
> #### `function loader(loaderCode) {return ''}`

#### `loader-utils`这个第三方库，可以解析`options`中配置的对象

```
module.exports = {
    module: {
        rules: [
         {
             test: /\.js$/,
             use: './utils/loader1.js' /*这个use后面的值最后会用require()来引入：require("./utils/loader1.js")*/
         }
        ]
    }
}
```