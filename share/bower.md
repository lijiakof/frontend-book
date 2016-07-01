# Bower
Bower 是由 Twitter 推出的前端包管理工具，它类似于 Java 中的 Maven 对 jar 包的管理一样，Bower 是来管理 js、css 等前端包之间的依赖关系。

## 安装
因为是基于 Node，所以需要先安装 Node 以及 Npm。然后全局安装 Bower：

```
$ npm install bower -g
```

## 使用
### 初始化

```
$ bower init
```

```
? name (bower) bower-demo
? description a demo of bower
? main file
? what types of modules does this package expose?
? keywords bower
? authors (Jay <jay.li@baichanghui.com>)
? license (MIT)
? homepage index.html
? set currently installed components as dependencies? (Y/n)
...
```

这样它会自动生成一个 bower.json 的文件，里面描述了相关的项目信息和依赖信息：

```
{
  "name": "bower-demo",
  "authors": [
    "Jay <jay.li@baichanghui.com>"
  ],
  "description": "a demo of bower",
  "main": "",
  "moduleType": [],
  "keywords": [
    "bower"
  ],
  "license": "MIT",
  "homepage": "index.html",
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ]
}
```

### 安装依赖

```
$ bower install jquery
```

```
bower not-cached    https://github.com/jquery/jquery-dist.git#*
bower resolve       https://github.com/jquery/jquery-dist.git#*
bower checkout      jquery#3.0.0
bower progress      jquery#* Receiving objects:  19% (27/140)
bower progress      jquery#* Receiving objects:  74% (104/140), 332.00 KiB | 12.00 KiB/s
bower resolved      https://github.com/jquery/jquery-dist.git#3.0.0
bower install       jquery#3.0.0

jquery#3.0.0 bower_components/jquery
```

### 删除依赖

```
$ bower uninstall jquery
```

### 安装不同的版本

```
$ bower install jquery#1.7.2
```

### 更新依赖

```
$ bower update jquery
```

### 查看依赖

```
$ bower list
```

```
bower check-new     Checking for new versions of the project dependencies...
bower-demo /Users/lijia/Desktop/bower
└── jquery#3.0.0 extraneous
```

### 查看本地 bower 已经缓存的类库

```
$ bower cache list
```

```
angular=git://github.com/angular/bower-angular.git#1.2.28
angular=git://github.com/angular/bower-angular.git#1.3.6
angular=git://github.com/angular/bower-angular.git#1.4.4
angular=git://github.com/angular/bower-angular.git#1.4.7
```

### 查看某一类库的信息

```
$ bower info jquery
```

```
bower jquery#*                  cached https://github.com/jquery/jquery-dist.git#3.0.0
bower jquery#*                validate 3.0.0 against https://github.com/jquery/jquery-dist.git#*

{
  name: 'jquery',
  main: 'dist/jquery.js',
  license: 'MIT',
  ignore: [
    'package.json'
  ],
  keywords: [
    'jquery',
    'javascript',
    'browser',
    'library'
  ],
  homepage: 'https://github.com/jquery/jquery-dist',
  version: '3.0.0'
}

Available versions:
  - 3.0.0
  - 2.2.4
  - 2.2.3
  - 2.2.2
  - 2.2.1
```

### 查看类库地址
 
```
$ bower lookup jquery
```

```
jquery https://github.com/jquery/jquery-dist.git
```

### 搜索类库

```
$ bower search jqu
```

```
jquip https://github.com/mythz/jquip.git
jq https://github.com/jquery/jquery.git
jQuery https://github.com/jquery/jquery.git
jqurey https://github.com/components/jquery.git
jquery https://github.com/jquery/jquery-dist.git
jQueue https://github.com/raincious/jQueue.git
```








