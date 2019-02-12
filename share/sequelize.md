# Sequelize 入门
Sequelize 是一个基于 promise 的 Node.js ORM, 目前支持 Postgres, MySQL, SQLite 和 Microsoft SQL Server. 它具有强大的事务支持, 关联关系, 读取和复制等功能。

* 安装
* 建立连接
* 数据模型
* 查询

## 安装
这是 js 的常规步骤：

```
$ npm install --save sequelize

# 还有以下之一:
$ npm install --save pg pg-hstore
$ npm install --save mysql2
$ npm install --save sqlite3
$ npm install --save tedious // MSSQL

// Or
$ yarn add sequelize

# 还有以下之一:
$ yarn add pg pg-hstore
$ yarn add mysql2
$ yarn add sqlite3
$ yarn add tedious // MSSQL
```

## 建立连接
Sequelize 通过数据库的地址、用户名、密码、库名 来连接数据库。如果从单个进程连接到数据库，你最好每个数据库只创建一个实例，如果要从多个进程连接到数据库，则必须为每个进程创建一个实例，但每个实例应具有“最大连接池大小除以实例数”的最大连接池大小。因此，如果您希望最大连接池大小为90，并且有3个工作进程，则每个进程的实例应具有30的最大连接池大小。

```
const Sequelize = require('sequelize');
const Koa = require('koa');

const sequelize = new Sequelize('database', 'username', 'password', {
    host: 'localhost',
    dialect: 'mysql', //'mysql'|'sqlite'|'postgres'|'mssql'
    pool: {
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
    }
});

sequelize
    .authenticate()
    .then(() => {
        console.log('Connection successfully.');
    })
    .catch(err => {
        console.error('Unable to connect to the database:', err);
    });

```

## 数据模型
我们通过 `sequelize.define` 来创建数据库模型，有了模型后，程序的对象就可以和数据库的表结构形成对应关系了。

```
const model = sequelize.define('user', {
    name: Sequelize.STRING,
}, {
    freezeTableName: true,    
    tableName: 'USER',
    timestamps: false
});
```

## 查询
查询操作是数据库最常用的操作，Sequelize 提供了强大的查询方式，让用户不用编写 SQL 语句就能完成复杂的查询：

```
let users = await model.findAll({
    limit: 10,
});
```