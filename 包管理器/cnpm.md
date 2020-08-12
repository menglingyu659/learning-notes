## cnpm

> ### 官网地址：[cnpm官网](https://npm.taobao.org/)

##### 为了解决国内用户连接 npm registry 缓慢的问题，淘宝搭建了自己的 registry，即淘宝镜像源

##### 过去，npm没有提供修改registry的功能，因此，淘宝提供了一个`CLI`工具`cnpm`（最开始其实就是一个发布在 npm 上面的一个包叫做 cnpm），它支持除了 `npm publish` 意外的所有命令，只不过连接的是淘宝镜像源

##### 如今，npm 已经支持修改 registry 了，可能cnpm唯一的作用就是和 npm 共存，即如果要使用官方源，就用`npm`，如果使用淘宝源 就用 `cnpm`