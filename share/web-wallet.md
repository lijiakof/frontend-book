# WEB 数字钱包的实现
最近区块链风靡互联网行业，个人也专门了解了相关技术，也买过数字货币，当然也有数字钱包。一直在思考这种去中心化的钱包，到底安不安全，原理是个啥：

* 我们的钱包密码（私钥）是不是被这些 App 偷偷摸摸的把它劫获了？
* 各大交易平台中，我购买的数字货币到底在哪儿？
* 区块链中到底有没有用户的钱包信息？

带着这些问题，我们要研究一下数字钱包到底是咋回事？先搞清楚一些概念：

## 加密 & 对称加密 & 非对称加密
加密学在区块链技术中属于核心技术之一，钱包的生成也是由加密算法来完成的，当然如果讲述加密技术对我这个不专业的人来说不能讲述的非常明白，不过我们从对它的功能上来大概的了解一下相关概念。

通俗的讲：
* 加密：把东西（信息）用钥匙锁在箱子里面，只有拿到钥匙的人才能使用这个东西（查看信息）；
* 对称加密：关闭箱子的钥匙和打开箱子的要是一样；
* 非对称加密：关闭箱子的钥匙和打开箱子的不一样；

专业的讲：
* 加密：将明文信息改变为难以读取的密文内容，使之不可读。只有拥有解密方法的对象，经由解密过程，才能将密文还原为正常可读的内容；
* 对称加密：加密和解密时使用相同的密钥；
* 非对称加密：需要两个密钥来进行加密和解密；

### 为什么要用非对称加密，应用场景在哪儿？
其安全性更好：对称加密的通信双方使用相同的秘钥，如果一方的秘钥遭泄露，那么整个通信就会被破解。而非对称加密使用一对秘钥，一个用来加密，一个用来解密，而且公钥是公开的，秘钥是自己保存的，不需要像对称加密那样在通信之前要先同步秘钥。

但是，非对称加密的缺点是加密和解密花费时间长、速度慢，只适合对少量数据进行加密。

## 签名

## 加密算法

## 钱包需要什么功能？
钱包的核心功能：

* 创建钱包 & 导入钱包
* 查看钱包的资产
* 发生交易

## 相关 Js 库

* ethereumjs-util
    * bn.js：BigNumber 数据类型
    * safe-buffer：Buffer 二进制数据类型
    * create-hash: Hash 算法
    * secp256k1：椭圆曲线算法
    * keccak：SHA-3 算法
    * rlp：RLP 编码
    * ethjs-util
* ethereumjs-wallet
* ethereumjs-tx
* ethereumjs-abi
* web3

## 钱包的创建（私钥、公钥、地址）
创建钱包我们可以采用 `ethereumjs-wallet` 库来完成，它是基于 椭圆曲线的ECDSA 算法来创建密钥对的。看源码：

* 公钥生成：(ethereumjs-wallet)generate() -> (ethereumjs-util)privateToPublic -> secp256k1.publicKeyCreate -> publicKey
* 地址生成：(ethereumjs-wallet)generate() -> (ethereumjs-util)privateToAddress -> (ethereumjs-util)sha3 -> (keccakjs)SHA3 -> address

```
// ethereumjs-wallet 模块
Wallet.generate = function (icapDirect) {
    if (icapDirect) {
        while (true) {
            var privKey = crypto.randomBytes(32)
            if (ethUtil.privateToAddress(privKey)[0] === 0) {
                return new Wallet(privKey)
            }
        }
    } 
    else {
        return new Wallet(crypto.randomBytes(32))
    }
}

var Wallet = function (priv, pub) {
    //...
}

Object.defineProperty(Wallet.prototype, 'pubKey', {
  get: function () {
    if (!this._pubKey) {
        this._pubKey = ethUtil.privateToPublic(this.privKey)
    }
        return this._pubKey
    }
})

// ethereumjs-util 模块
exports.privateToAddress = function (privateKey) {
    return exports.publicToAddress(privateToPublic(privateKey))
}

var privateToPublic = exports.privateToPublic = function (privateKey) {
    privateKey = exports.toBuffer(privateKey)
    // skip the type flag and use the X, Y points
    return secp256k1.publicKeyCreate(privateKey, false).slice(1)
}

exports.pubToAddress = exports.publicToAddress = function (pubKey, sanitize) {
    pubKey = exports.toBuffer(pubKey)
    if (sanitize && (pubKey.length !== 64)) {
        pubKey = secp256k1.publicKeyConvert(pubKey, false).slice(1)
    }
    assert(pubKey.length === 64)
    // Only take the lower 160bits of the hash
    return exports.sha3(pubKey).slice(-20)
}

exports.sha3 = function (a, bytes) {
    a = exports.toBuffer(a)
    if (!bytes) bytes = 256

    var h = new SHA3(bytes)
    if (a) {
        h.update(a)
    }
    return new Buffer(h.digest('hex'), 'hex')
}
```

### 对于使用者非常简单：
相关文档：https://github.com/ethereumjs/ethereumjs-wallet

```
const ethUtil = require('ethereumjs-util')
const Wallet = require('ethereumjs-wallet');

// 生成钱包
var wallet = Wallet.generate();
var privateKey = wallet.getPrivateKey(); // 返回 Buffer，可以通过 wallet.getPrivateKeyString() 直接得到字符串
var publicKey = wallet.getPublicKey(); // 返回 Buffer，可以通过 wallet.getPublicKeyString() 直接得到字符串
var address = wallet.getAddress(); // 返回 Buffer，可以通过 wallet.getAddressString() 直接得到字符串

// 导入钱包
var privateKey2 = ethUtil.toBuffer('0xe601e598111629240e4dc6ec7a95534e025838bd0f638dabad9ad4152d80443b');
var wallet2 = Wallet.fromPrivateKey(privateKey2);
var publicKey2 = wallet2.getPublicKey();

```

## 钱包交易
钱包交易的过程：

* 构造交易数据
* 交易签名
* 发送交易

### 交易对象

```
{
    nonce: '0x00',
    gasPrice: '0x01',
    gasLimit: '0x01',
    to: '0x633296baebc20f33ac2e1c1b105d7cd1f6a0718b',
    value: '0x00',
    data: '0xc7ed014952616d6100000000000000000000000000000000000000000000000000000000',
    // EIP 155 chainId - mainnet: 1, ropsten: 3
    chainId: 3
}
```

参考以太坊文档：

### 构建 data
这一过程相对复杂，可以参考[Ethereum Contract ABI](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI)：

* 对调用合约函数的**函数名**进行签名
* 对调用合约函数的**参数**进行编码
* 

### 签名
ethereumjs-tx


```

```


## 钱包其它功能