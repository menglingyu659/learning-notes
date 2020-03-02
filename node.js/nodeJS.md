## NodeJS 
> ### 一般用于制作网站，接口。实时app，中间层，微服务



##### 特性：
- ###### 非阻塞I/O
- ###### 事件驱动
---
### 全局模块
#### buffer

###### *buffer：是八位字节组成的类数组，可以有效地在js中存储二进制数据,数据是16进制的*

```
//定义一个长度为10的buffer且长度不可变
const buffer1 = Buffer.alloc(10) 

//直接根据数据来创建buffer
const buffer2 = Buffer.from([1,2,3])
const buffer3 = Buffer.from('asfasf')

//写入
buffer.wirte(data:String)

//读取 --- toString()里面的参数为定义编码方式
buffer1.toString('ascii')//根据ascii表格来读取数据
buffer2.toString()//默认为uft-8方式来读取数据

//合并
const buffer4 = Buffer.concat([buffer1, buffer2, buffer3])
```
---
### 内联模块

#### fs模块（用于读取文件等）；

##### 异步读取文件

```

const fs = require('fs');
fs.readFile('文件的地址(eg:index.js)', callback); //eg：fs.readFile(“./index.js”, (err, data) => {})//
```
> 将fs模块promise化

- ~①使用node的内联模块~ ~util~

```
const { promisify } = require('util');
const readFile = promsify(fs.readFile);
readFile('./index.js').then();

```
- ~②如果node版本在10.X.X以上~

```
cosnt fsp = require('fs').promises;
fsp.readFile('./index.js').then()
```

##### 同步读取文件

```
const fs = require('fs');
const data = fs.readFileSync('./index.js');//data为读取到的内容
```


#### http模块：开启服务容器

```
const http = require('http');
const fs = require('fs');
const path = require('path')

//创建服务器
const server = http.createServer((req, res) => {
    //res.statusCode = 200 设置状态码为200;
    //res.setHeader('Content-Type', 'text/html') 告诉浏览器用什么方式来解码
    //res.end() 将数据返回给浏览器，参数为要传递的数据
    //req.url 查看请求的url;
    //req.method 查看请求的方法（get ， post等）;
    if (req.url === '/' && req.method === "GET") {
        fs.readFile(path.resolve('./index.html'), (err, data) => {
            if (err) {
                res.statusCode = 500;
                res.end('500 服务器出错了!')
            };
            res.statusCode = 200;
            res.setHeader('Content-Type', 'text/html');
            res.end(data);
        })
    }
})

//监听端口号
server.listen(3000);
```

#### 创建流操作（读取流，写入流），并且对于二进制文件可以直接将读取的文件写入到另一个文件中 
     

```
const fs = require('fs');

//创建一个读取流
const rs = fs.createReadStream('./index.js');

//创建一个写入流
const ws = fs.createWriteStream('./index1.js');

//将读取流连接到写入流（这样就会将index.js中的内容复制到index1.js中）
rs.pipe(ws)

```


#### 关于同源


```
//服务器常用到的设置的响应头
res.writeHead(200, {
    "Access-Control-Allow-Origin": "Http://locahost:3000",//允许请求的地址
    "Access-Control-Allow-Headers":"X-Token, Content-Type",//允许携带的参数（X-Token），允许修改Content-Type
    "Access-Control-Allow-Methods": "GET", //允许的请求方法
    "Access-Control-Allow-Credentials": "true" //如果要携带cookie，则请求变为crediential请求（证书形式的请求）要加上这个响应头
})
```


浏览器发出的请求分为两种：1.简单请求， 2.含有预检测的请求
1. 响应简单请求：方法为**GET/POST/HEAD**的请求，没有自定义请求头，**Content-Type**为 ==**application/x-www-form-urlencoded**==，==**multipart/form-data**==，==**text/plain**== 之一，该请求解决跨域的方法：

```
res.setHeader('Access-Control-Allow-Origin', "http://localhost:3000")
```
2. 响应preflight请求：该请求解决跨域的方法：

```
else if (method === 'OPTIONS' && url === '/users') {
    res.writeHead(200, {
    "Access-Control-Allow-Origin": "Http://locahost:3000",//允许请求的地址
    "Access-Control-Allow-Headers":"X-Token, Content-Type",//允许携带的参数（X-Token），允许修改Content-Type
    "Access-Control-Allow-Methods": "GET" //允许的请求方法
})
}
```




























































