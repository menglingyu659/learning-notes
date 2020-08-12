## npm 全称为 node package manager

### ==Node中读取json文件可以直接用`const jsonObj = require("package.json")`，读取到的jsonObj 就是一个json对象==

### ==mocha：开发时测试用的==

> #### npm之所以要运行在node环境，而不是浏览器环境，根本原因是因为浏览器环境无法提供下载、删除、读取本地文件的功能。而node属于服务器环境，没有浏览器的种种限制，理论上可以完全掌控运行node的计算机



### npm组成

- ##### registry：入口（npm的registry部署在国外）

  - ###### 可以把它想象成一个庞大的数据库

  - ###### 第三方库的开发者，将自己的库按照`npm`规范，打包上传到数据库中

  - ###### 使用者通过统一的地址下载第三方包

- ##### 官网：[npm](https://www.npmjs.com)

  - ###### 查询包

  - ###### 注册、登录、管理个人信息

- ##### CLI：command-line    interface   命令行接口

  - ###### 这一部分是这门课讲解的重点

  - ###### 安装好npm后，通过CLI来使用npm的各种功能

> #### node和npm是互相成就的

> #### npm由于服务器在国外，所以下载包的时候会特别慢，需要将npm配置到国内镜像，淘宝国内镜像地址：https://registry.npm.taobao.org，配置方式为：`npm config set registry https://registry.npm.taobao.org`
>
> #### 如果本地安装的包带有`CLI`，`npm`会将它的`CLI`脚本文件放置在node_modules/.bin下，直接在命令行运行CLI命令时不行的，因为在当前环境变量下找不到这个命令，使用`npm`提供的`npx`即可成功使用。
>
> #### 当有npx时，默认将当前文件夹下面的node_modules/.bin文件夹作为当前执行的环境变量，它会去当前目录下的找寻node_modules/.bin目录下面找相应的CLI命令==eg:`npx mocha`==



### 全局安装和本地安装

#### 本地安装

```
npm install <package name>
```

#### 全局安装

```powershell
#安装
npm install <package name> -g
#查询全局安装的包所在的目录
npm config get prefix
```

##### ==注意：==全局安装的包并非所有工程可用，它仅提供全局的`CLI`命令，这时就可以直接使用`mocha`这个命令，而可以不用`npx mocha`。



### 包的配置

#### 目前遇到的问题：

1. ##### 拷贝工程后包如何还原

2. ##### 如何区分开发以依赖和生产依赖

3. ##### 如果自身的项目也是一个包，如何描述包的信息

##### 以上问题都需要同通过包的==配置文件==解决



### 配置文件：名称固定为`package.json`

##### `npm`将每个使用`npm`的工程本身看作是一个包，包的信息需要一个名称固定的==配置文件==来描述

##### `package.json`：可以手动创建该文件，更多的是通过 `npm init` 命令来创建

#### 配置文件（package.json）的内容

- ##### name：包的名称，必须是*英文*，支持链接符

- ##### version：版本

  - 版本规范：主版本号.次版本号.补丁版本号
  - 主版本号：仅当程序发生了重大变化是才会增长，如技术架构发生调整
  - 次版本号：仅当程序发生了一些小的改变时才会增长，如新增了一些小功能
  - 补丁版本号：仅当解决了一些bug 或 进行了一些局部代码优化时

- ##### description：描述

- ##### homepage：官网地址

- ##### author：包的作者，必须是有效的npm账户名，书写规范是`account <<mail>`>，==eg：`zhangsan<zhangsan@gmail.com>`==，不正确的账号和邮箱可能导致发包失败

- ##### repository：包的仓储地址，通常指`git`或`svn`的地址，他是一个对象

  - type：仓储地址类型，`git`或`svn`
  - url：地址

- ##### main：包的入口文件，使用包的人默认从该入口文件导入包的内容

- ##### keywords：搜索关键字，发布包后，可以通过该数组中的关键字搜索到包

- ##### license：协议

- ##### ==dependencies==：生产环境的依赖包

- ##### ==devDependencies==：仅在开发环境的依赖包

- ##### ==scripts==：命令行脚本

使用`npm init --yes`或`npm init -y`可以在生成配置文件时自动填充默认配置



### 安装`package.json`中的依赖

##### 生产环境和开发环境的依赖包都安装

```
npm i
```

##### 仅安装生产环境的依赖

```powershell
npm i --production
```



### 包的使用

##### nodejs对npm支持非常良好



### 包的寻找

```js
const _ require("lodash");
//1.找node_modules目录下的package.json文件的main属性值，根据这个配置的入口来寻找入口文件。如果没有main属性则使用index.js作为入口文件

const a = require('./a')
//1.读取当前文件夹下面的a.js，若没有
//2.读取a文件夹下面的package.json文件的main字段，若没有
//3.读取a文件夹下面的index.js文件
```



### 语义版本

> ##### ^：次版本号和补丁版本号可最新
>
> ##### ~：补丁版本号可更新
>
> ##### *：也可以写成`latest`，安装最新版本



### 避免还原依赖时造成的差异

> #### 用package-lock.json的原因：
>
> - ##### 当包的次版本和补丁版本可以增加时，版本的增加修改了其他bug和增加一些新的API，但却产生了新的问题，这个问题又恰巧影响到了我的项目。
>
> - ##### 如果不允许版本增加，包就不能自我优化
>
> - ##### 使用的包的版本增加，该包的依赖的版本也增加，这样又会带来更多不可控的问题

> #### 使用package-lock的好处和弊端
>
> - ##### 好处：使用这个文件当运行`npm install`时，它会先读取这个文件，并安装所有依赖的确切版本（不做包版本的升级）
>
> - ##### 弊端：因为安装的时确切版本，所以包不能得到优化

#### ==在生产上或者开发中是否需要这个文件（package-lock.json）来使用确切版本，主要看你自己的团队选择==

##### `package-lock.json`：记录每一个依赖包之间的确切关系，并控制每一个依赖包的确切版本



### npm的差异版本处理

##### 平面依赖（flat dependency）：如果a依赖b，往常的做法是在node_modules下的a目录下存放b，现在做法是将b也放在node_moudles下面和a平级

> #####  问题：a	——>【c 1.4.0】
>
> ##### 		    b	——>【c 1.2.3】，
>
> 上面这种情况node_modules目录就会是，c在a的目录下版本是1.4.0 。c在b的目录下版本为1.2.3
>
> ```
> |—-node_modules
> |  |—-a
> |  |  |—-node_modules
> |  |  |  |--c 1.4.0
> |  |  |  |  |--c包的文件
> |  |  |--a包的文件
> |  |--b
> |  |  |--node_modules
> |  |  |  |--c 1.2.3  
> |  |  |  |  |--c包的文件
> |  |  |--b包的文件
> ```



### npm脚本（npm scripts）

##### ==细节==

- ##### 配置在脚本(scripts)上的命令可以省略`npx，可写可不写，因为写在脚本上的命令默认当前文件夹下面的node_modules/.bin为执行的环境变量。

- ##### `stop、start、test`可以省略`run`，eg：`npm start`

- ##### `start`脚本如果没有配置，当使用`npm start`命令时，会运行`node server.js`命令



### 运行环境配置

#### 一般存在三种运行环境：

##### 1. 开发环境

##### 2. 生产环境

##### 3. 测试环境

##### 设置环境变量NODE_ENV的方法

###### 1. 永久设置：此电脑（右键点击）——> 环境变量 ——> 系统变量 ——> 添加

###### 2. 临时设置：可以通过命令行设置，一般将其配置在package.json文件的scripts属性上作为脚本，来运行。widnow:`set NODE_ENV=development`，mac：`export NODE_ENV`

> ##### ==以为运行的计算机操作系统不同设置命令方式也不同，所以需要一个第三方库`cross-env`来辅助==
>
> ##### `cross-env`使用：
>
> ```js
> //1.命令行安装依赖包，安装后会在当前环境下提供一个CLI，供使用
> npm i cross-env -D
> //2.package.json中配置 
> {
> 	"scripts": {
> 		"build": "cross-env NODE_ENV=development node index.js"  //不要&&连接
> 	}
> }
> ```
>
> ##### ==不使用==`cross-env`的写法:
>
> ```js
> //window下
> {
> 	"scripts": {
> 		"build": "set NODE_ENV=development&&node index.js"
> 	}
> }
> //mac下
> {
> 	"scripts": {
> 		"build": "export NODE_ENV=development&&node index.js"
> 	}
> }
> ```
>
> 

##### node中有一个全局属性process.env可以获取所有系统环境变量

> ```js
> //index.js
> //通过设置系统环境变量名字为NODE_ENV，值为development就可以将这个属性添加到process.env中
> console.log(process.env.NODE_ENV)//development   console.log()  development
> ```



### 其他npm命令

- ##### 安装

- ##### 查询

- ##### 更新

- ##### 卸载

- ##### npm配置

#### 安装

1. ##### 精确安装最新版本

```powershell
#在package.json文件中版本不再是 ^1.x.x，而是1.x.x
npm i -E <package name> 
#或者
npm i --save-exact <package name>
```

2. ##### 安装指定版本

```powershell
npm install <packageName@version>
```

#### 查询  []中的内容代表可选

1. ##### 查询包安装路径

```powershell
npm root [-g]
```

2. ##### 查询包信息，未安装的包也可以查询

```powershell
npm view <packageName> [子信息:versions]
#view aliases: v info show
```

3. ##### 查询安装包，安装的包

```powershell
npm list [-g] [--depth=依赖深度:--depth-3]  #依赖深度表示所查询安装包的指定深度的依赖包是哪一个安装包
#list aliases: ls la ll
```

#### 更新

1. ##### 检查有哪些包需要更新

```powershell
npm outdated
```

2. ##### 更新包

```powershell
npm update [-g] [packageName]  #更新包
#update aliases: up upgrade
```

#### 卸载

```powershell
npm uninstall [-g] <packageName>
#al: remove rm r un unlink
```



### npm配置

> ##### 安装好npm之后，最终会产生两个配置文件，一个是==用户配置==，一个是==系统配置==，当两个文件的配置项有冲突的时候，==用户配置会覆盖系统配置==
>
> ##### 通常我们不去关心具体的配置文件，而只关心最终生效的配置

##### 查询生效的所有配置

```powershell
npm config ls [-l或者--json] #-l查询所有的配置
```

##### 查询某个配置

```
npm config get <配置项>
```

##### 设置某个配置

```
npm config set <配置项=值>
```

##### 移除某个配置

```
npm config delete <配置项>
```



### 发布包

> #### 包的名字不能出现：==大写==，==中文==，==空格==，==字符==

#### 准备

1. #### 移除淘宝镜像源（防止将包发布在淘宝镜像官网上，应该将包发布在npm的官网上）

2. #### 到npm官网注册一个账号，并完成邮箱认证

3. #### 本地使用`npm CLI`进行登录

   - ##### 使用命令`npm login`登录

   - ##### 使用命令`npm whoami`查看当前登录的账号

   - ##### 使用命令`npm logout`注销

4. #### 创建工程根目录

5. #### 使用`npm init`初始化目录

#### 发布

- #### 命令`npm publish`完成发布