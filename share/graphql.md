# GraphQL 极速指南
一种用于 API 的查询语言：GraphQL 既是一种用于 API 的查询语言也是一个满足你数据查询的运行时。 GraphQL 对你的 API 中的数据提供了一套易于理解的完整描述，使得客户端能够准确地获得它需要的数据，而且没有任何冗余，也让 API 更容易地随着时间推移而演进，还能用于构建强大的开发者工具。

* 安装
* 使用
    * Hello world
    * Schema
    * Type
* 插件

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

// 请求所需数据
graphql(schema, '{ hello }', root).then((response) => {
    // 得到结果
    console.log(response);
});
```

### Schema
`Schema` 是用来描述 API 的整个结构以及结构直接的关系，它看起来很像 JSON 数据结构，但是在描述能力上比 JSON 更加的强大，并且内置了很多类型，可以定义变量、参数等等。以下是一个简单的 `Schema`：

```
type User {
    id: ID
    name: String!
}
```

### Type
`Schema` 有完善的类型系统，通过这些类型，我们可以描述出千变万化的数据：

* 对象类型
* 字段
* 查询&变更类型
* 标量类型
* 枚举类型
* ...

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
    name: String!
    gender: Gender
}
```

* `User` 是**对象类型**
* `name` 和 `gender` 是 `User` 上的**字段**
* `String` 是内置**标量类型**之一
* `String!` 表示 `name` 字段是**非空的**
* `[User]!` 表示 **User 数组**，它也是**非空的**
* `Gender` 表示**枚举类型**

当然 GraphQL 可以运行在任何后端框架或者编程语言之上，我们只会专注在类型系统如何描述可以查询的数据，具体的数据实现由各个服务端去完成内部的逻辑。

## 插件
* express-graphql
* egg-graphql
* apollo-server
    * apollo-server-express
    * apollo-server-koa
    * apollo-server-hapi
    * apollo-server-lambda
    * apollo-server-azure-functions
    * apollo-server-cloud-functions
    * apollo-server-cloudflare
* apollo-client
* graphql-request
* grafoo