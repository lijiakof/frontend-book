# Jest
Jest 是由 Facebook 出品的 JavaScript 单元测试框架，官方声称通过它可以完成“令人愉快的 JavaScript 测试”。

## 快速入门

### 安装：

* yarn: `yarn add --dev jest`
* or npm: `npm install --save-dev jest`

### 被测试的模块 `hello.js`：

```
function hello() {
    return 'Hello world!';
}

module.exports = hello;
```

### 创建测试文件 `hello.test.js`：

```
const hello = require('./hello');

test('hello', () => {
    expect(hello()).toBe('Hello world!');
});
```

### 配置 `package.json`

```
{
    //...
    "scripts": {
        "test": "jest",
        "coverage": "jest --coverage"
    }
    //...
}
```

### 运行 `yarn test` 开始测试：

```
PASS ./hello.test.js
```

### 生成覆盖率：

```
jest --coverage
```

以上就是你的第一个 Jest 单元测试。

## Babel
通过安装 `babel-jest` 和 `regenerator-runtime` 可以让 Jest 支持 ES6 的新语法：

```
yarn add --dev babel-jest babel-core regenerator-runtime
```

### 配置 `.babelrc`

```
{
    "presets": [
        "es2015",
        "stage-2"
    ]
}
```

## 其它：
更多内容参考相关文档：https://facebook.github.io