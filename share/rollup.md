# Rollup
Rollup 官方声称是下一代 ES 模块打包工具。现在有很多打包工具，当然最为流行的是 Webpack，对于 Web 应用来说 Webpack 是不错的选择，但是对于开发类库来说 Rollup 是一个不错的选择。

## 安装
同样通过 NPM 很容易，我们也可以通过 Yarn 来安装

```
npm install --global rollup
OR
yarn yarn global add rollup
```

## 使用

### 应用程序入口 main.js

```
// main.js
import foo from './foo.js';
export default function () {
  console.log(foo);
}
```

### 依赖模块 foo.js

```
// foo.js
export default 'hello world!';
```

### 构建

```
$ rollup src/main.js -o bundle.js -f cjs
```

上面的命令通过 -f 选项告诉编译器采用 CommonJS 方式构建输出到 bundle.js 文件中去：

```
'use strict';

var foo = 'hello world!';

var main = function () {
  console.log(foo);
};

module.exports = main;
```

## rollup.config.js
对于复杂的构建，有太多的选项需要配置，这样一来通过命令行对我们来说太过复杂，我们可以通过配置文件来描述打包的过程：

在项目根目录创建文件 `rollup.config.js`

```
// rollup.config.js
export default {
    input: 'src/main.js',
    output: {
        file: 'bundle.js',
        format: 'cjs'
    }
};
```

通过命令 `rollup -c` Rollup 会执行上述配置。

## 插件
我们使用 `rollup-plugin-json` 插件来读取 JSON 文件。

### 安装插件

```
yarn add rollup-plugin-json -D
```

### 在代码中使用插件

```
// src/main.js
import { version } from '../package.json';

export default function () {
  console.log('version ' + version);
}
```

### 使用插件

```
// rollup.config.js
import json from 'rollup-plugin-json';

export default {
    input: 'src/main.js',
    output: {
        file: 'bundle.js',
        format: 'cjs'
    },
    plugins: [ json() ]
};
```

### 编译结果：

```
'use strict';

var version = "1.0.0";

var main = function () {
  console.log('version ' + version);
};

module.exports = main;
```

**注意：只有我们实际需要的数据——name 和 devDependencies 和 package.json 中的其它数据被忽略了。这是 tree-shaking 起了作用。**

## ES6
通常我们需要用高版本的语言来编写代码，Rollup 可以借助 `rollup-plugin-babel` 插件来完成：

### 安装插件及 babel

```
yarn add rollup-plugin-babel -D
yarn add babel-core babel-preset-env babel-preset-es2015 babel-preset-stage-2 -D
```

### 配置 rollup

```
// rollup.config.js
import json from 'rollup-plugin-json';
import babel from 'rollup-plugin-babel';

export default {
    input: 'src/main.js',
    output: {
        file: 'bundle.js',
        format: 'cjs'
    },
    plugins: [ 
        json(),
        babel({
            exclude: 'node_modules/**' 
        }) 
    ]
};
```

### 配置 `.babelrc`
在项目根目录上增加配置 babel 配置文件：`.babelrc`

```
{
    "presets": ["es2015", "stage-2"]
}
```

这样一来，你就可以编译 ES6 语法了。

## 总结
Rollup 使用起来非常简单，另外它通过 `Tree-shaking` 技术去除无用代码，让整个项目更佳的高效，最后它是一个非常好的类库构建工具，如果再借助 Jest 进行单元测试，我们就非常好的开发 JS 类库了。

## 参考
* https://rollupjs.org
