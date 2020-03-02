## Node.js ORM --- Sequelize


- ##### 概述： 基于Promise的ORM（Object Relation Mapping），支持多种数据库，事务，关联等
- ##### 安装： npm install sequelize mysql2 --save 
- ##### 基本使用：

```
    const Sequelize = require('sequelize');
    
    //建立连接
    const sequelize = new Sequelize('test', 'root', 'admin', {
        host: "localhost",
        dialect: "mysql", //使用哪种数据库语言
        opeartorsAliases: false
    })
    
    //1.定义模型 Model - Table  第三个参数为配置项
    const Fruit = sequelize.define('fruit'//定义表的名称, {
        name: Sequelize.STRING(20), //定义表中的数据和数据对应的配置
        price: {type: Sequelize.FLOAT, allowNull: false}
    }, {
        freeTableName: true //正常表的名字会自动变成复数（设置fruit会自动变成fruits），如果这个设置为true，就按照自己写的名字来定义不会变复数
    })
    
    //2.将模型同步到数据库
    Fruit.sync()
    
    //强制同步
    Fruit.sync({force: true})/*会将原来表格清空重新同步数据*/
    
    //添加数据
    Fruit.sync().then(() => {
        return Fruit.create({name: '香蕉', price: 3.5});//添加数据的API
    }).then(() => {
        Fruit.findAll()/*查询数据的API*/.then(fruits => {
            //fruits就是这个表所有的数据（findAll查询所有数据）
        })
    })//这个then是处理上一个then返回的Promise对象（因为Fruit.create()返回Promise对象）
```