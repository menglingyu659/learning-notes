##### ==DOM更新了不代表浏览器重新渲染了，有可能仅仅只是dom这个js对象更新了==

#### 浏览器在读取`js`代码时，如果遇到会改变`ui`布局的代码，会先将这个变化存储在`GUI`线程。当遇到异步的js代码==（如定时器，或者宏任务类的）==时，异步代码的回调函数会与`GUI`线程抢夺时间线==（因为js线程和gui线程互斥）==，哪一个快先运行哪一个线程。但是在js代码中如果遇到强制发生重排的代码，会停止`js`线程的运行，先运行`GUI`线程，运行`GUI`线程时会将所有记录在`GUI`线程的变化也一并重新排列==（包括js代码导致的变化）==

举例：**希望让一个盒子，在没有动画的情况下向右移动100px并且颜色变红，然后加上transition动画，让这个盒子在恢复到最初的状态**

```html
<html lang="en">
  <head>
    <title>Document</title>
    <style>
      .transitioned {
        transition: all 1s linear;
      }

      .change {
        transform: translate(100px);
        color: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <p>p</p>
      <button>change</button>
    </div>
 ×  <script>  //这个代码会在js全部运行完成之后，计算最终的ui变化：{添加了一个类名transitioned}，change添加又删除最终为不变，所以没有动画  
      const oBtn = document.getElementsByTagName("button")[0];
      const oP = document.getElementsByTagName("p")[0];
      oBtn.onclick = () => {                  
        oP.classList.add("change");
        oP.classList.add("transitioned");
        oP.classList.remove("change");
      };
    </script>    
√   <script>  //在运行时先添加change类名，遇见document.body.offsetWidth发生重排，会先运行gui线程，并将添加change类名的变化渲染到页面，然后添加transitioned类名，并删除change类名
      const oBtn = document.getElementsByTagName("button")[0];
      const oP = document.getElementsByTagName("p")[0];
      oBtn.onclick = () => {
        oP.classList.add("change");
        document.body.offsetWidth;
        oP.classList.add("transitioned");
        oP.classList.remove("change");
×       //oP.classList.add("change");     //不能实现这个功能，原因未知
        //oP.classList.add("transitioned");
        //document.body.offsetWidth;
        //oP.classList.remove("change");
      };
    </script>
√   <script>   // setTimeout慢于添加change类名的这次gui渲染（有可能会快于但我没有复现出，‘快’这个情况），渲染了两次，第一次添加类名change，第二次添加transitioned，删除change
      const oBtn = document.getElementsByTagName("button")[0];
      const oP = document.getElementsByTagName("p")[0];
      oBtn.onclick = () => {
        oP.classList.add("change");
        setTimeout(() => {
          oP.classList.add("transitioned");
          oP.classList.remove("change");
        });
      };
    </script>
×   <script>  //宏任务Promise.then快于添加change类名的gui渲染，只渲染了一次页面      只放开注释1.可以实现，只放开注释2.或3.不能实现
      const oBtn = document.getElementsByTagName("button")[0];
      const oP = document.getElementsByTagName("p")[0];
      oBtn.onclick = () => {
        oP.classList.add("change");
        Promise.resolve().then(() => {
√         //1.document.body.offsetWidth;
          oP.classList.add("transitioned");
×         //3.document.body.offsetWidth;
          oP.classList.remove("change");
×         //2.document.body.offsetWidth;
        });
      };
    </script>
  </body>
</html>

```



==只有在发生重排之后，同时添加transitioned和change类名才能实现==，即使先添加transitioned这个类名也不行。

以下写法均不可以，原因未知：

```js
  oBtn.onclick = () => {
    oP.classList.add("change");
    oP.classList.add("transitioned");
    setTimeout(() => {
      oP.classList.remove("change");
    });
  };
  oBtn.onclick = () => {
    oP.classList.add("change");
    Promise.resolve().then(() => {
      oP.classList.add("transitioned");
      document.body.offsetWidth;
      oP.classList.remove("change");
    });
  };
```

