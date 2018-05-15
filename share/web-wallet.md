# WEB 数字钱包的实现
最近区块链风靡互联网行业，个人也专门了解了相关技术，也买过数字货币，当然也有数字钱包。一直在思考这种去中心化的钱包，到底安不安全：

* 我们的钱包密码（私钥）是不是被这些 App 偷偷摸摸的把它劫获了？
* 个大交易平台中，我购买的数字货币到底在哪儿？
* 区块链中到底有没有用户的钱包信息？

带着这些问题，我们要研究一下数字钱包到底是咋回事？先搞清楚一些概念：

## 加密 & 对称加密 & 非对称加密
加密学在区块链技术中属于核心技术之一，钱包的生成也是由加密算法来完成的，当然如果讲述加密技术对我这个不专业的人来说不能讲述的非常明白，不过我们从对它的功能上来大概的了解一下相关概念。

通俗的讲：
* 加密：把东西（信息）用钥匙锁在箱子里面，只有拿到钥匙的人才能使用这个东西（查看信息）；
* 对称加密：关闭箱子的钥匙和打开箱子的要是一样；
* 非对称加密：关闭箱子的钥匙和打开箱子的不一样；

专业的讲：
* 加密：利用技术手段把重要的数据变为乱码（加密）传送，到达目的地后再用相同或不同的手段还原（解密）；

### 为什么要用非对称加密，应用场景在哪儿？

## 签名

## 加密算法

## 相关 Js 库

* secp256k1
* crypto
* keccak
* ethjs-util
* bn.js
* ethereumjs-tx
* tweetnacl
* elliptic
* buffer
* web3

## 钱包的创建（私钥、公钥、地址）

## 钱包交易

## 钱包其它功能