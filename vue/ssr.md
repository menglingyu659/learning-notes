## VUE——SSR

### 概念

- #### 服务端渲染：将`vue`实例渲染为HTML字符串直接返回，在前端激活为交互程序

### 优点

- #### 利于SEO

- #### 首屏优化

### 基础实现

- #### 使用渲染器将vue实例渲染成HTML字符串并返回

- #### 安装相关依赖

  ```shell
  yarn add vue-server-renderer vue //两者版本要相同
  ```

- #### 步骤——这种方法传到前端用户是不能交互的`methods上绑定的事件不能被触发`==(因为传到前端的是字符串)==

  ```jsx
  const Vue = require("vue");
  const { createRenderer } = require("vue-server-renderer");
  const express = require("express");
  
  const app = express();
  const renderer = createRenderer();
  
  app.get("/", async (req, res) => {
    const vm = new Vue({
      template: `
          <div>
              <b>{{message}}</b>
          </div>
          `,
      data: {
        message: "I'm vue ssr",
      },
      methods: {
        click() {
          console.log("click");
        },
      },
    });
    try {
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