## VUE——SSR（在服务端渲染，在客户端激活，传统vue的单页面应用是在客户端渲染）

### 概念

- #### 服务端渲染：将`vue`实例渲染为HTML字符串直接返回，在前端激活为交互程序

### 优点

- #### 利于SEO

- #### 首屏优化

### 基础实现

- #### 使用渲染器将vue实例渲染成HTML字符串并返回

- #### 安装相关依赖

  ```shell
  yarn add vue-server-renderer vue vue-router//两者版本要相同
  ```

- #### 步骤——这种方法传到前端用户是不能交互的`methods上绑定的事件不能被触发`==(因为传到前端的是字符串)==，`router-link和router-view可以正常使用`

  ```jsx
  const Vue = require("vue");
  const { createRenderer } = require("vue-server-renderer");
  const express = require("express");
  const Router = require("vue-router");
  Vue.use(Router);
  const app = express();
  const renderer = createRenderer();
  
  app.get("*", async (req, res) => {
    const router = new Router({
      mode: "history",
      routes: [
        {
          path: "/",
          component: { template: "<div>index page</div>" },
        },
        {
          path: "/detail",
          component: { template: "<div>detail page</div>" },
        },
      ],
    });
    const vm = new Vue({
      template: `
          <div>
              <b>{{message}}</b>
              <router-link to="detail">detial</router-link>
              <router-view></router-view>
          </div>
          `,
      data: {
        message: "I'm vue ssr",
      },
      router,
      methods: {
        click() {
          console.log("click");
        },
      },
    });
    try {
      router.push(req.url); //首屏页面渲染
      const html = await renderer.renderToString(vm);
      res.send(html);
    } catch (error) {
      res.status(500);
      res.send("Internal Server Error 500");
    }
  });
  app.listen(8800);
  ```

### 单页面应用——客户端渲染（通过js编译再渲染到页面上）

#### 优点

- ##### 渲染计算再放到客户端

- ##### 省流量

### 缺点

- ##### 不利于SEO

- ##### 首屏时间长



### 同构开发SSR应用

#### 对于同构开发依然使用`webpack`打包，我们要解决两个问题：

1. #### 服务端首屏渲染

2. #### 客户端激活

### 完整实现

#### 新建工程

```
vue craete ssr
```

#### 安装依赖

```
yarn add vue-server-renderer vue //两者版本要相同
```

#### 启动脚本

```

```

