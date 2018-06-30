# 数字钱包从产品到发布

## 用户故事
需求源于一个故事，多个故事形成一个产品。

## 用户功能
数字钱包，作为一个数字资产交易的工具，产品 Owner 需要做一些相关功课，至少要玩转一下各种不同的钱包 App，总结出产品的一些功能，当然可以对基本功能进行更多的扩展：

* 初始化钱包
    * 创建钱包
    * 导入钱包
        * 私钥导入
        * 助记词导入
        * Keystore 导入
        * 钱包地址导入（观察钱包）
    * 导出钱包
        * 私钥导出
        * 助记词导出
        * Keystore 导出
* 查询资产
    * ETH 总个数
    * Token 总个数
    * 交易列表查询
    * 交易详情
* 转账交易
    * ETH 转账
    * Token 转账
    * 调用合约
* 个人中心
    * 密码管理
    * 钱包管理
    * 联系人管理
    * 交易管理...
* 其它
    * 货币行情

当然如果采用 Scrum 敏捷方式来开发这个产品，我们要遵循 MVP（Minimum Viable Product） 原则，已最快的速度上线一个可用的版本，以上所有功能显然太过于庞大，以至于项目时间会拖得很长，我们会从中摘取最重要的功能：

* 创建钱包
* 私钥导入钱包
* ETH & Token 总资产
* 交易列表
* ETH & Token 转账
* [Low]交易详情
* [Low]调用合约

看起来只有 7 个主要页面，此时我们需要和极客们讨论产品的可行性和未来的方向，

## [技术]模块
* transactionObject
    * [contract]
    * [methodName]: String
    * [arguments]: Array
    * [privateKey]: String
    * from: String
    * [to]: String
    * [value]: 
    * [gasLimit]
    * [gasPrice]
    * [data]
    * [none]
* attributes
    * privateKey
    * publicKey
    * address
    * currency
* methods
    * generate([currency]): Object
    * import(key [, type] [, currency]): Object
        * type: 'privateKey', 'keystore', 'mnemonicPhrase', 'readonly'
        * key: string
        * currency: string
    * setProvider(host)
    * getBalance(addressHexString): Promise
    * getTokenBalance(addressHexString, contractAddress): Promise
    * getTransaction(transactionHash): Promise
    * contract(abi, address): Object
    * estimateGas(transactionObject): Promise
    * gasPrice(): Promise
    * sendTransaction(transactionObject): Promise
* storage
    * 私钥如何存储

## 原型

## 流程图


