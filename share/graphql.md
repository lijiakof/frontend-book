# GraphQL
一种用于 API 的查询语言：GraphQL 既是一种用于 API 的查询语言也是一个满足你数据查询的运行时。 GraphQL 对你的 API 中的数据提供了一套易于理解的完整描述，使得客户端能够准确地获得它需要的数据，而且没有任何冗余，也让 API 更容易地随着时间推移而演进，还能用于构建强大的开发者工具。

* 安装
* 使用
    * Hello world
    * Type
    * Schema

## 安装

```
npm install graphql
Or
yarn add graphql
```

## 使用

### Hello world
* 描述数据：buildSchema()
* 请求所需数据：graphql(schema, '{ hello }', root)
* 得到结果：console.log(response)

```
var { graphql, buildSchema } = require('graphql');

// 描述数据
var schema = buildSchema(`
    type Query {
        hello: String
    }
`);

var root = { hello: () => 'Hello world!' };

graphql(schema, '{ hello }', root).then((response) => {
    console.log(response);
});
```

### Type
* 对象类型
* 字段
* 查询&变更类型
* 标量类型
* 枚举类型

```
type Query {
    user(id: ID): User
    users: [User]!
}

enum Gender {
    FEMALE
    MALE
}

type User {
    id: ID
    name: String!,
    gender: Gender
}
```

* `User` 是**对象类型**
* `name` 和 `gender` 是 `User` 上的**字段**
* `String` 是内置**标量类型**之一
* `String!` 表示 `name` 字段是**非空的**
* `[User]!` 表示 **User 数组**，它也是**非空的**
* `Gender` 表示**枚举类型**

### Schema
`Schema` 是用来描述