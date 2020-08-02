## yarn

### 安装yarn

> #### 最开始`yarn`是通过`npm`命令来安装的，yarn最开始是`npm`的一个包

#### 现在安装方式：

##### 进入yarn官网：[yarn官网](www.yarnpkg.com)  ——>  选择window  ——>  Download Installer



### yarn的出现解决了哪些npm存在的问题

1. #### npm包的目录嵌套太深，在windows系统上会出现问题，==（因为windows系统，目录长度最多能支持`256bytes`，eg: d:a/b/c/xxx，只能写256个字节==

2. #### 下载速度慢：

   - ##### 由于包的目录是嵌套的所以不能同时下载，只有先下载完外层的包，才能下载内层的包

   - ##### 版本相同的包被重复下载

3. #### 控制台输出繁杂

   - ##### 过去npm安装吧会将大量包的相关信息输出，当遇到错误时，很难调试

4. #### 工程移植问题

   - ##### 由于npm的版本依赖可以是模糊的（没有package-lock.json文件），可能导致工程移植后，依赖的版本不一致，带来一些问题



### yarn解决npm存在问题的方案

1. #### 使用扁平的目录结构，只有当一个项目依赖同一包的多个版本时才使用嵌套

2. #### 并行下载

3. #### 使用本地缓存（在本地的yarn全局文件夹中有一个缓存文件，会将所有下载过的包都缓存到这个文件中）

4. #### 控制台仅输出关键信息（错误信息等）

5. #### 使用`yarn-lock.json`文件记录确切的依赖版本

#### 不仅如此，yarn还优化了以下内容：

1. ##### 增加了某些功能强大的命令（后面会讲)

2. ##### 让已有的命令更加语义化

3. ##### 本地（局部安装不是全局安装）安装的`CLI`工具可以使用`yarn`直接启动，现在npm是通过npx来运行，以往npm是通过进入node_modules下面的.bin目录然后在运行`mocha`等`CLI`命令

4. ##### 将全局安装的目录当作一个普通的工程，生成`package.json`文件，以便全局安装移植



### 目前npm6版本解决了以上所有问题

1. #### 目录扁平化

2. #### 并行下载

3. #### 本地缓存（在本地的`npm-cache目录下`缓存依赖)

   - ##### 清空该缓存：`npm cache clean`

4. #### 使用`package-lock.json`文件记录依赖确切版本

5. #### 增加大量的命令别名

6. #### 内置`npx`，可以使用局部的CLI命令工具

7. #### 极大简化了控制台的输出



### yarn的核心命令

1. #### 初始化

```powershell
yarn init [--yse/-y]     #-y：所有配置全同意
```

2. #### 安装依赖

```powershell
安装指定包： yarn [global](要想全局安装global只能写在add前面) add <packageName> [--dev/-D] [--exact/-E] #-E：
安装package.json中的所有依赖：yarn / yarn install [--production/--prod](只安装生产环境依赖)
```

3. #### 运行脚本

```powershell
yarn start; yarn run dev  #stop start test 可以省略run
运行局部CLI：yarn run CLI
```

4. #### 查询

   - ##### 查询yarn的配置

   ```powershell
   yarn config list
   ```

   - ##### 查看bin目录

   ```powershell
   yarn [global] bin
   ```

   - ##### 查询包信息（未安装的包也可查询）

   ```powershell
   yarn info <packageName> [子字段:version]  #子字段是readme可以将说明文档读取出来
   ```

   - ##### 查询所有已安装依赖

   ```powershell
   yarn [global] list [--depth=依赖深度]
   ```

   > ##### yarn的list命令和npm的list命令不同，yarn输出的信息更加丰富，包括顶级目录结构、每个包的依赖版本号

5. #### 更新

   - ##### 列举需要更新的包

   ```powershell
   yarn outdated
   ```

   - ##### 更新包

   ```powershell
   yarn [global] upgrade [packageName]
   ```

6. #### 卸载

```powershell
yarn remove <packageName>
```



### yarn的特别礼物

1. #### `yarn check`

   ##### 使用`yarn check`命令，可以验证`package.json`文件的依赖记录和`package-lock.json`文件的依赖版本是否一致

   ##### *这对于防止篡改非常有用*

2. #### ==`yarn audit`：公司开发很常用的漏洞==

   ##### 可以检查本地安装的包有哪些已知漏洞，以表格的形式列出，漏洞级别分为以下几种：

    - ##### INFO：信息级别

    - ##### LOW：低级别

    - ##### MODERATE：中级别

    - ##### HIGH：高级别

    - ##### CRITICAL：关键级别

3. #### `yarn why <packageName>`

    ##### 可以在控制台打印出为什么安装了这个包，哪些包会用到它

4. #### `yarn create`

   ##### *非常有趣的命令*

   ##### *脚手架相关的命令*

   ##### 平时使用脚手架来搭建工程的做法：

   1. ###### 全局安装脚手架工具

   2. ###### 使用全局脚手架CLI工具搭建工程

   ##### 由于大部分脚手架工具都是以`create-xxx`方式来命名的，比如`react`官方脚手架命令`create-react-app`

   ##### 因此，可以使用`yarn create`命令来进一步完成安装和搭建

   ##### 例如：

    ```powershell
    yarn create react-app my-app
    #这个命令等同于以下两条命令
    yarn global add create-react-app
    create-react-app my-app
    ```





