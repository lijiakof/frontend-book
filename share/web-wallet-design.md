# WEB 数字钱包设计

## 用户功能
* 初始化钱包
    * 创建
    * 导入
        * 私钥导入
    * 导出
* 查询资产
    * 多少个 ETH
    * 交易查询
* 转账交易
    * 转出 ETH
    * 调用合约
* 个人中心
    * 密码管理
    * 钱包管理
    * 联系人管理
    * 交易管理...
* 其它
    * 货币行情

## 模块
* transactionObject
    * nonce
    * from
    * to
    * value
    * gas
    * gasPrice
    * data
    * contract（会覆盖 data 数据）
        * methodName
        * paramsName[]
        * paramsValue[]
* wallet
    * privateKey: string
    * publicKey: string
    * address: string
    * generate([currency]): wallet
    * import(key [, type] [, currency]): wallet
        * type: 'privateKey', 'keystore', 'mnemonicPhrase', 'readonly'
        * key: string
        * currency: string
    * // export(pwd [, currency])
    * private encodeAbi(methodName, paramsName[], paramsValue[]): string
    * private signTx(transactionObject): string
    * getBalance(addressHexString [, defaultBlock] [, callback])
    * getTransaction(transactionHash [, callback])
    * send(transactionObject[, callback])
* storage
    * 私钥如何存储
* market

## 原型

## 流程图


