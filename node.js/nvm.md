## nvm

#### 它是用于管理多个 `node` 版本的工具

#### 场景：

##### 2年前： 使用 node v8版本

##### 如今： 使用 node v12版本

#### 为了在一个计算机上管理多个node版本，所以需要使用 nvm 来切换计算机上node的版本



#### 下载和安装

##### 下载地址：[nvm下载地址](https://github.com/coreybutler/nvm-windows/releases)

##### 下载nvm-setup.zip后，直接安装



#### 使用nvm

##### nvm提供了`CLI`工具，用于管理node版本

##### 在终端中输入`nvm`，以查看各种可用命令

> ##### 为了加快下载速度，建议设置淘宝镜像源
>
> ###### [node淘宝镜像源](https://npm.taobao.org/mirrors/node/)
>
> ###### [npm淘宝镜像源](https://npm.taobao.org/mirrors/npm/)
>
> ##### 使用`nvm install`命令改变node版本时，它会去node的github地址安装，它会安装两个东西，一个是node本身，还有一个是node对应的npm版本
>
> ```powershell
> #设置nvm下载node时候，要使用淘宝镜像源
> nvm node_mirror https://npm.taobao.org/mirrors/node/
> #设置nvm的下载npm时，要使用淘宝镜像源
> nvm npm_mirror https://npm.taobao.org/mirrors/npm/
> ```
>



### 原理

##### 运行`nvm root`可以查看下载node各个版本的本地目录，运行`nvm use <version>`命令切换版本的时候，相当于把这个root目录中相应版本的node文件复制安装到node的安装目录并运行

> ##### 在切换到不同的node版本时，对应的全局安装的npm包也不同



#### 卸载：`nvm unstall <v>`就是在root目录下将对应版本文件夹删除

