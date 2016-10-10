# Webpack
现在 Web 前端发展得非常快，项目也越来越庞大，项目所依赖的模块也越来越多，我们需要一个帮助我们管理依赖并将依赖一起打包的工具。市面上的确已经有了非常优秀的像 Grunt 和 Gulp 这样的自动化构件工具，但是我们又为什么要重复制造轮子呢：

## 为什么重复造轮子
以下是 Webpack 指南的原话：

这些已有的模块化工具并不能很好的完成如下的目标：

* 将依赖树拆分成按需加载的块
* 初始化加载的耗时尽量少
* 各种静态资源都可以视作模块
* 将第三方库整合成模块的能力
* 可以自定义打包逻辑的能力
* 适合大项目，无论是单页还是多页的 Web 应用

## 安装
当然首先要安装 [Node.js](https://nodejs.org/en/)；

### 全局安装 Webpack

```
$ npm install webpack -g
```

可以通过如下命令尝试是否安装 OK：

```
$ webpack -h
```

### 项目中安装 Webpack

进入到项目目录，初始化项目后：

```
$ npm install webpack --save-dev
```

## 使用
### 创建入口页面和入口程序：

```
<!-- index.html -->
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf8">
    <title>Webpack</title>
</head>

<body>
    <script src="bundle.app.js"></script>
</body>

</html>
```

```
<!-- app.js -->
document.write('It works.');
```

### 编译打包
运行如下命令，打开 index.html 会看到 `It works.`：

```
$ webpack src/app.js dist/bundle.app.js
```

### 多个模块打包

添加一个新模块，并在 app.js 中引用此模块：


```
<!-- module.js -->
module.exports = 'It works from module.js';
```

```
<!-- app.js -->
var module = require('./module.js');

document.write('It works.');
document.write(module);
```

再次运行打包命令，打开 index.html 会看到 `It works.It works.It works from module.js`。

Webpack 会分析入口文件，解析包含依赖的各个文件，这些模块都会打包到 bundle.app.js 中去。

## Loader
Loader 是模块和资源的转换器，Webpack 只会处理 JavaScript 模块，如果要处理其它类型的文件就需要靠 Loader 来做转换。我们接着上面的例子：

### 安装

```
$ npm install css-loader style-loader --save-dev
```

### 创建 CSS 文件

```
/* style.css */
body { color: red; }
```

### 在程序中引入

```
<!-- app.js -->
var module = require('./module.js');
require("./style.css") // 引入 style.css

document.write('It works.');
document.write(module);
```

### 编译打包
运行如下命令，打开 index.html 会看到红色的 `It works.It works.It works from module.js`：

```
$ webpack src/app.js dist/bundle.app.js --module-bind 'css=style!css'
```

## 配置文件
当然我在打包的过程中会有各种不同的情况需要处理，如果通过这种命令行来处理显得有写困难，并且难以理解。为了方便我们可有通过配置来完成这个事情。在项目的根目录创建配置 `webpack.config.js` ：

```
var webpack = require('webpack');
module.exports = {
    entry: './app.js',
    output: {
        path: __dirname,
        filename: 'bundle.app.js'
    },
    module: {
        loaders: [
            {test: /\.css$/, loader: 'style!css'}
        ]
    }
}
```

这样一来，就可以直接通过命令 `webpack` 来打包了。

## 插件
Webpack 插件可以完成更多的 Loader 无法完成的功能，例如：`html-webpack-plugin` 可以自动的创建 html 模版，并且将依赖的最终产物自动的添加的 html 内部，我们尝试一下：

### 安装插件

```
$ npm install html-webpack-plugin --save-dev
```

### 在配置中使用

```
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    entry: './src/app.js',
    output: {
        path:'./dist',
        filename: 'bundle.app.js'
    },
    module: {
        loaders: [
            {test: /\.css$/, loader: 'style!css'}
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        })
    ],
}
```

修改 `index.html` 去除中间的 js 的引用：

```
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf8">
    <meta name="viewport" content="minimal-ui,width=device-width,initial-scale=1.0, maximum-scale=1.0,minimum-scale=1.0,user-scalable=no" />
    <title>Webpack</title>
</head>

<body>
    
</body>

</html>
```

这样在此运行 `webpack` 命令，打包结束后会发现 `dist` 文件夹中自动的创建了一个 `index.html` 并且把一来的 `bundle.app.js` 自动添加了进去。

## webpack-dev-server
借助 `webpack-dev-server` 会让我们的开发环境更加的简单，它可以创建一个 web 服务，并且可以监听程序的变化并自动刷新页面。

### 安装

```
$ npm install webpack-dev-server --save-dev
```

### 配置

```
// ...

module.exports = {
    // ...
    devServer: {
        inline: true,
        progress: true,
        port: 8080,
        historyApiFallback: true
    }
    // ...
}
```

### 运行

运行如下命令后，在浏览器中打开：http://localhost:8080/ 就可以看到你的页面了：

```
$ $(npm bin)/webpack-dev-server
```

