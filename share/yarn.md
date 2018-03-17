# Yarn
Yarn 发布于2016年10月，是 Facebook、Google、Exponent 和 Tilde 开发的一款新的 JavaScript 包管理工具。它相比与 npm 更佳的高效、安全和可靠。在 Github 上迅速拥有了2.4万个 Star，而 npm 只有1.2万个 Star。

## Yarn 的优势
* 高效：Yarn 缓存了每个下载过的包，所以再次使用时无需重复下载。 同时利用并行下载以最大化资源利用率，因此安装速度更快。
* 安全：在执行代码之前，Yarn 会通过算法校验每个安装包的完整性。
* 可靠：使用详细、简洁的锁文件格式和明确的安装算法，Yarn 能够保证在不同系统上无差异的工作。

## 如何安装（macOS）
我们通过 `Homebrew` 来安装 Yarn，如果没有安装 Node 它也可以安装：

```
brew install yarn
yarn --version
```

升级 Yarn

```
brew upgrade yarn
```

## 如何使用
### 初始化新项目

```
yarn init
```

### 添加依赖包

```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]

yarn add [package] --dev
yarn add [package] --peer
yarn add [package] --optional
```

### 升级依赖包

```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

### 移除依赖包

```
yarn remove [package]
```

### 安装项目的全部依赖

```
yarn
或者
yarn install
```

## Yarn 的工作流
* 创建一个新项目：Yarn 也是用 package.json 文件来管理项目的依赖的
* 增加／更新／删除依赖
* 安装／重装你的依赖
* 引入版本控制系统
    * package.json：包含包的所有依赖信息；
    * yarn.lock：记录每一个依赖项的确切版本信息；
* 持续集成

## yarn.lock
* 为了跨设备安装得到一致的结果，Yarn 需要比 package.json 中更多的依赖信息，Yarn 使用 yarn.lock 文件来保存每一个安装的依赖版本；
* yarn.lock 文件是自动生成的，而且应该完全被 Yarn 管理。当你在对项目进行 增加／更新／删除 等依赖操作时，它会自动的更新 yarn.lock 文件；
* 所有的 yarn.lock 文件都应该被提交到版本控制系统中去，这样才会让所有的设备保持同样的依赖。

## 如何从 npm 迁移到 Yarn
大家不要担心迁移，从 npm 迁移到 Yarn 是一件非常简单的事情，Yarn 和 npm 使用同样的 package.json 文件，现在只需要运行 `yarn` Yarn 将通过自己的解析算法来重新组织 `node_modules` 目录，这个算法和 node.js 模块解析算法是兼容的。

## 参考
* https://www.sitepoint.com/yarn-vs-npm/