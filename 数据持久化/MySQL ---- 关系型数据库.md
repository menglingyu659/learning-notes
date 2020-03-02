## MySQL ---- 关系型数据库

##### 1. 电脑安装MySQl
##### 2. 安装mysql模块   npm install mysql --save
##### 3. 引入mysql模块

#### 用js来创建数据库：~mysql~模块没有~promise~化，可以用~co-mysql~模块来解决下面的~回调地狱~的问题
```
    const mysql = require('mysql');
    
    //写mysql的配置项
    const cfg = {
        host: localhost, //数据库的地址
        user: 'root', //数据库的用户名（可以自己设置，如果不设置默认为root）
        password: 'admin', //密码（电脑安装数据库时设置的密码）
        database: 'test', //创建的数据库的名称
    };
    
    //创建连接对象
    const conn = mysql.createConnection(cfg);
    
    //连接数据库
    conn.connect(err => {
        //err参数可以判断链接是否成功，如果err存在说明不成功
        if (err) {
            throw err //如果不成功抛出这个错误err
        }
        console.log('连接成功')
    })
    
```

> 以下是原声SQL语句，和上面是连续的，我单独开一个代码块: 我一共写了三个步骤

```
    //创建表的SQL语句
    const CREATE_SQL = `CREATE TABLE IF NOT EXISTS test(
        id INT NOT NULL AUTO_INCREMENT,
        message VARCHAR(45) NULL,
        PRIMARY KEY(ID)
    )`;
    
    //插入数据的SQL语句 “?” 为占位符
    const INSERT_SQL = `INSERT INTO test(message) VALUES(?)`;
    
    //查询数据的SQL语句
    const SELECT_SQL = `SELECT * FROM test`
    
    conn.connect(err => {
        //err参数可以判断链接是否成功，如果err存在说明不成功
        if (err) {
            throw err //如果不成功抛出这个错误err
        }
        console.log('连接成功');
        //1.创建表
        conn.query(CREATE_SQL, err => {
            if (err) throw err;
            
            // 2.插入数据
            conn.query(INSERT_SQL, "要出入的数据来替换上面的?占位符", (err, result) => {
                if (err) throw err;
                console.log(result);
                
                // 3.查询数据
                conn.query(SELECT_SQL, (err, datas) => {
                    console.log(datas);
                    conn.end();
                });
                
            });
            
        })
    })
    
```





















