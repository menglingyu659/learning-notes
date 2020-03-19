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
>> ```js
>> //a.js
>> export const a = 'a'
>> export const b = 'b'
>> export default 'c'
>> //b.js
>> const obj = require('./a.js');/*相当于--->*/  import * as obj from './a.js'
>> //obj {a:'a',b:'b',default:'c'}
>> ```

> ##### commonJS export -- es6 import： 
>> ```js
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


```js
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

#### 将一种代码，转换成另一种代码

> #### `function loader(loaderCode) {return ''}`

#### `loader-utils`这个第三方库，可以解析`options`中配置的对象

```js
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



## plugin

> #### 完成下面的功能需要plugin
> > - #### 当某一个时候需要做什么事情，需要plugin来完成
> >
> > - #### 大部分`webpack`生成文件时，顺便多生成一个说明描述文件
> >
> > - #### 当`webpack`编译启动时，控制台输出一句话表示已经启动
> >
> > - #### 将某一个功能嵌入到webpack编译过程中



```js
//plugin.js
module.exports = class Plugin {
    apply(compiler) { /*apply是钩子函数*/
        console.log('webpack运行了')
    }
}

//index.js
const Plugin = require('./plugin.js')
module.exports = {
    plugin: [
        new Plugin()
    ]
}
```

> #### `apply(compiler) {}`这个方法会在初始化创建好compiler对象的时候触发，并接受compiler对象作为参数
> > #### compiler对象是在初始化阶段创建的，整个webpack打包期间只有一个compiler对象，后续完成打包工作的是compiler对象内部创建的compilation对象
> >
> > #### apply方法会在创建好compiler对象后调用，并向方法中传入一个compiler对象



## 区分环境

- #### 可以写两个配置文件 , 在命令行用`npx webpack --config webpack.xxx.js`来指定使用哪一个配置文件

> - #### 使用同一套配置,在配置文件中导出一个函数
> >`npx webpack --env abc`对应函数中的`env`就是`“abc”`
> >
> >`npx webpack --env.abc`对应函数中的`env`就是`{abc: true}`
> >
> >`npx webpack --env.abc=1`对应函数中的`env`就是`{abc:1}`
> >
> >`npx webpack --env.abc=1 --env.w=2`对应函数中的`env`就是`{abc:1, w:2}`
> >
> >```js
> >module.exports = env => {
> >    return {}
> >   }
> >```
> >
> >



## webpack 常用扩展

1. #### clean-webpack-plugin: 清楚输出目录

2. #### html-webpack-plugin: 自动生成页面（html文件），并且html文件会自动引入css，js文件

3. #### copy-webpack-plugin: 将某一个文件夹下面的文件复制到另一个文件夹下，一般html模板会引入一个img图片，因为在你打包的时候并没有用require（）引入这个图片路径，所以在编译的时候这个图片就不会配打包，这时候就需要这个插件，来将这个图片，复制到你想复制到的路径下

4. >#### webpack-dev-server: 开发服务器，在  *开发阶段*   开启的一个服务器
   >>##### 既不是loader，也不是plugin，是webpack官方单独出的一个库
   >>
   >>##### 这个库依然是按照`webpack.config.js`的配置来开启服务，自带watch功能，并会自动刷新页面
   >>
   >>##### 使用：1.安装`yarn add webpack-dev-server`   2. 启动：`webpack-dev-server`
   >>
   >>##### 这个命令并不会打包生成文件到output，它会保存资源列表（assets）并开启一个服务器，可以直接访问localhost来访问页面，所以只有在  *开发阶段*   使用，真正  *生产阶段*   还是要用webpack来打包文件
   > #### 配置
   > > ```js
   > > module.exports = {
   > >     devServer: {
   > >         port: 8899 //服务器监听的端口号，
   > >         open: true //开启之后自动打开浏览器窗口并访问URL地址，
   > >         index: 'index.html' //默认访问的html页面地址,
   > >         stats: {} //控制命令行窗口的输出信息
   > >         proxy: {  //代理请求地址
   > >         '/api' : {
   > >         target: 'https://xxx.xxx.com',
   > >         changeOrigin: true /*将请求的请求头的host和origin也一并变为转发后的URL的host和origin*/
   > >    			 }
   > >    		} 
   > >     }
   > > }
   > > ```

5. #### file-loader: 当导入文件的时候，可以生成一个文件放入output目录中去，并导出一个文件路径

6. #### url-loader: 将一个引入的文件转换，并导出一个base64格式的文件

7. > #### 解决file-loader和url-loader遇到的路径问题：
   > >```js
   > >module.exports = {
   > >    output: {
   > >       publicPath: '/'   //一般配置成"/"，生成的js文件相当于，配置一个字符串给 __webpack_require__.p，一般的插件和loader会将这个字符串，写在路径的前面做拼接 
   > >    }
   > >}
   > >```

8. #### 常用webpack内置插件（`DefinePlugin`,`BannerPlugin`,`ProvidePlugin`）

   ##### 使用：

   ```js
   const webpack = requrie("webpack");
   new webpack.[PluginName](options)
   ```

  - ##### DefinePlugin: 	在开发的时候我们可以直接用这些常量（PI,VERSION,DOMAIN）,webpack在编译的时候会将这些常量替换成后面的值（Math.PI,...）

    ```js
    module.exports = {
        plugins: {
            new webpack.DefinePlugin({ //定义常量
            	PI: `Math.PI`, //相当于const PI = Math.PI
            	VERSION: `"1.0.0"`, //const VERSION = "1.0.0"
            	DOMAIN: JSON.stringify('attain.com')/*或者写成*/ DOMAIN: `"attain.com"`
        	})
        }
    }
    ```

    在源码中：

    ```js
    console.log(PI);
    console.log(DOMAIN);
    console.log(VERSION);
    ```

    

- #### BannerPlugin: 可以为每一个chunk生成的文件头部添加一行注释，一般用于添加公司，作者，版权等信息

  ```js
  module.exports = {
      plugins: {
          new webpack.BannerPlugin({
          	banner: `hash: [hash]
  					chunkhash: [chunkhash]
  					name: [name]
  					author: menglingyu
  					`
      	})
      }
  }
  ```

- ##### ProvidePlugin: 自动加载模块，而不必导出  `import` 和  `require`

  ```js
  module.exports = {
      plugins: {
          new webpack.ProvidePlugin({
          	$: 'jquery',
          	_: 'lodash'
      	})
      }
  }
  ```

  在源码中：

  ```js
  $(`#root`) //可以直接用jqurey中的$对象
  _.drop([1, 2, 3], 4)
  ```




## 其他细节配置

- #### context：入口模块路径上下文

- #### output

  - ##### library :导出全局

  - ##### libraryTarget： 导出方式（var）

- #### target：模式（web，node）

- #### module

  - ##### noParse: `noParse: /jquery/` 不编译正则表达式匹配的模块直接输出到js文件中，一般用于大型的，且模块内没有其他依赖的模块

- #### resolve

  - ##### modules：当有依赖模块的时候，从哪里寻找

  - ##### extensions: 引入模块时候，自动补全后缀名

  - ##### alias：

    ```js
    alias: {
    	"@": path.resolve(__dirname, 'src')
    }
    ```

- #### externals : 对于某个模块不进行打包（如果导出的js中`jquery`是用cdn方式引入的就不需要在编译的时候打包`jquery`，直接将`jquery`的所有代码替换成`$`(这个`$`不是字符串而是`jquery`cdn引入暴露给全局的$对象),但为了开发时候书写方便还是要require() ）

  ```
  externals: {
  	jquery: '$'
  }
  ```

  

- #### stats：控制编译过程中命令行的输出内容
















































