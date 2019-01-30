# Eslint
ESLint 最初是由 Nicholas C. Zakas 于2013年6月创建的开源项目。它的目标是提供一个插件化的javascript代码检测工具。

* 安装
* 使用
* 配置

## 安装
老规矩：

* Local
    * `npm install eslint --save-dev`
    * Or `yarn add eslint --dev`
* Global
    * `npm install -g eslint`
    * Or `yarn global add eslint`

## 使用
* 初始化
    * Local: `./node_modules/.bin/eslint --init`
    * Global: `eslint --init`
* 运行
    * Local: `./node_modules/.bin/eslint your.js`
    * Global: `eslint your.js`

## 配置
当运行初始化命令 `eslint --init` 后，将生成一个 `.eslintrc` 配置文件在当前文件夹中，类似于：

```
{
    "parser": "babel-eslint",
    "extends": "eslint:recommended",
    "env": { "node": true },
    "rules": {
        "indent": [ "error", 4 ],
        "quotes": [ "error", "single" ],
        "semi": 2,
        "no-unused-vars": 2,
        "no-console": 1,
        "no-debugger": 2
    }
}
```

配置：

* `"extends": "eslint:recommended"` 开启默认规则：https://eslint.org/docs/rules/
* `env` 环境配置
* `rules` 配置规则，它会覆盖默认规则
    * `indent`  强制使用一致的缩进
    * `quotes`  强制使用一致的反勾号、双引号或单引号
    * `semi`    要求或禁止使用分号代替 ASI
    * `no-unused-vars`  禁止出现未使用过的变量
    * `no-console`      禁用 console
    * `no-debugger`     禁用 debugger
    * 更多参考：https://eslint.org/docs/rules/

规则的值：
* `"off"` 或者 `0` ：关闭规则
* `"warn"` 或者 `1` ：将规则视为一个警告
* `"error"` 或者 `2` ：将规则视为一个错误

## Git Hook
通过 `pre-commit` 组件，可以通过 git hook 在开发人员提交代码前校验代码，保证提交到 Git 上的代码都是符合规范的。

* 安装：`yarn add pre-commit`
* 配置：`package.json`

```
{
    "name": "yourproject",
    "version": "0.1.0",
    "scripts": {
        "lint": "eslint --ext .js ./src --fix --cache"
    },
    "pre-commit": [
        "lint"
    ],
    "dependencies": {
    },
    "devDependencies": {
        "pre-commit": "^1.2.2"
    }
}
```