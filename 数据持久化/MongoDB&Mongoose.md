## MongoDB:`默认支持连接池`

### 可视化工具 `Robo3T`

#### 1. 下载
#### 2. 配置环境变量
#### 3. 创建dbpath文件
#### 4. 启动
> 在命令行运行 `mongod --dbpath=/data`

> `mongod --dbpath D:\mongdb1安装包\data\db //后面的是你安装时候的db所在的地址，db文件自己创建`

### mongodb原生驱动

##### 引入模块： `npm install mongodb --save`
##### 连接mongodb --- `mongodb语法`基本都很好的支持`promise化`，==~*操作一条用one，操作多条用many~==

```
//客户端
const MongoClient = require('mongodb').MongoClient;

//连接url
const url = 'mongodb://localhost:27017'; /*默认端口是27017*/
const url = 'mongodb://user:password@localhost:27017' /*如果有用户名和密码需要加上user和password字段*/

// 要连接的数据库名称
const dbName = 'test';


(async function () {
    //创建客户端
    const client = new MongoClient( url, {useNewUrlParser:true} );
    
    try {
        //1.连接数据库
        await client.connect();
        console.log('连接成功');
        
        //2.获取数据库
        const db = client.db(dbName);
        
        //3.获取集合
        const fruitsColl = db.collection('fruits');
        
        //4.插入文档，返回Promise
        let r = await fruitsColl.insertOne({name: '芒果', price: 20.0});
        console.log('插入成功', r.result);
        
        //5.查询文档
        r = await fruitsColl.findOne();
        console.log('查询结果', r);
        
        //6.更新文档
        r = await fruitsColl.updateOne({name}: '芒果', {$set: {name: '苹果'}});
        console.log('更新成功', r.result);
        
        //7.删除文档
        r = await fruitsColl.delete({name: '苹果'});
        console.log('删除成功'，r.result);
    } catch(error) {
        console.log(error);
    }
    
    
    client.close();/*关闭客户端，大可不必加上这句话*/
})()

```

##### 聚合操作符：使用aggregate方法，使文档顺序通过管道阶段从而得到最终结果

```
```

### Mongoose
##### 中文文档：http://www.mongoosejs.net/

##### 英文文档：https://mongoosejs.com/docs/index.html

##### 教程：https://mongoose.kkfor.com/

##### 菜鸟教程：https://www.runoob.com/mongodb/mongodb-tutorial.html


#### 操作符：

```
$or
$gt
$gte
$lt
$lte
$and
$regex
```