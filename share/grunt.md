# 自动化构建工具 Grunt
我们在开发过程中经常需要构建（Build），通常构建是一种重复性的工作，例如：编译 TypeScript 成 ES5JS、编译 SASS 成 CSS、合并脚本样式、压缩脚本和样式、改变文件结构、给文件重命名等等，这一系列的操作如果项目结构固定后基本是一成不变的操作；它好像是把各种食材（代码）倒入到搅拌机，最终经过搅拌机的搅拌成了最终的果汁（部署的程序），如果某种果汁的食谱定型了，搅拌的过程也会固定，这样只需要设定好相应的步骤就可以完成最终的产物。

当然这种重复性的工作我们都用这种流程化的工具来完成，现在有很多的自动化构建的工具 Grunt、Gulp 等等，这里主要讲的是 Grunt。

## 什么是 Grunt
Grunt 是前端世界的构建工具，它依托于 Node.js 的 npm 有着庞大的生态系统，你可以在众多的插件中像搭积木一样完成自己的构建，如果你找不到插件，也可以自己动手创造一个 Grunt 插件。

## 安装 Grunt
### [安装 Node.js](https://nodejs.org/en/)
Grunt 和 Grunt 插件是通过 npm 安装并管理的。npm 是一个 node 包管理和分发工具，已经成为了非官方的发布 node 模块（包）的标准。有了 npm ，可以很快的找到特定服务要使用的包，进行下载、安装以及管理已经安装的包。

#### 常用命令
* npm update -g
* npm install -g [model-name]

#### n 模块
n 模块是用来快速更新 node 版本的模块
* npm install -g n
* n stable 更新到最近的稳定版本
* n v0.10.26 更新到具体版本

#### nrm 模块
nrm 是一个 NPM 源管理器，允许你快速地在如下 NPM 源间切换
* npm install -g nrm
* nrm ls 
* nrm use taobao
* nrm add <registry> <url> [home]
* nrm del <registry>
* nrm test npm
* nrm test

## 安装 Grunt-CLI
运行 Grunt 之前需要安装 Grunt 命令行
* [sudo ]npm install -g grunt-cli
* [sudo ]npm uninstall -g grunt

## 安装 Grunt 和 Grunt 插件
安装指定插件并配置到 package.json 中：

```
[sudo ]npm install [module-name ]--save-dev
```

安装 package.json 中的所有插件：

```
[sudo ]npm install
```

初始化 Grunt 项目（Gruntfile.js）

```
[sudo ]npm init
```

## 使用
### 创建 Grunt 项目

```
//Gruntfile.js
moule.exports = function(grunt) {
    require('load-grunt-tasks')(grunt);

    require('time-grunt')(grunt);

    grunt.initConfig({
        //codes......
    });
}
```

### 创建一个拷贝任务

```
//Gruntfile.js
moule.exports = function(grunt) {
    //codes......

    grunt.initConfig({
        copy: {
            taskA: {
                src: 'a.html',
                dist: 'dist/a.html'
            },
            taskB: {
                src: ['b.html', 'b1.html'],
                dist: 'dist/b.html'
            }
        }
    });
}
```

### 执行任务
通过命令行进入到 Gruntfile.js 文件目录，执行`grunt TaskName`来运行对应的命令，例如执行上面的拷贝命令：

```
grunt copy:taskA
```

## Grunt 常用插件
* Js 文件校验：grunt-contrib-jshint
* 文件拷贝：grunt-contrib-copy
* 文件删除：grunt-contrib-clean
* 文件合并：grunt-contrib-concat
* Sass 解析：grunt-contrib-sass
* Css 压缩：grunt-contrib-cssmin
* Html 压缩：grunt-contrib-htmlmin
* Js 压缩混淆：grunt-contrib-uglify
* 静态资源文件模版：grunt-usemin
* 静态资源文件版本：grunt-rev
* 任务运行时间插件：time-grunt
* 加载 Grunt 插件：load-grunt-tasks
* ...

## 集成到 Sublime
配置 Sublime 的 Build System `Tools > Build System > New Build System Grunt.sublime.build` 

```
{
    "cmd": ["grunt", "--no-color”, “build"],
    "path": "/bin:/usr/bin:/usr/local/bin",
    "working_dir": "${project_path:${folder}}",
    "selector": ["Gruntfile.js"]
}
```


