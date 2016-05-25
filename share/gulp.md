# Gulp
Gulp 和 Grunt 工具一样，都是自动化构建工具，它相对于 Grunt 来说在性能和维护上来说有更大的优势，所以有很多前端开发开始转向 Gulp 作为他们的构建工具。Gulp 的插件也是相当丰富，让我们来看看 Gulp 是如何使用的。

## 安装 Gulp
同样它也是运行于 node 之上的，需要安装 Nodejs

### 全局安装 gulp

```
[sudo ]npm install gulp -g
```

### 作为项目的开发依赖

```
[sudo ]npm install --save-dev gulp
```

### 安装 Gulp 插件

```
[sudo ]npm install [module-name ]--save-dev
```

## 使用
### 在项目的根目录下创建`gulpfile.js`文件

```
var gulp = require('gulp');

gulp.task('default', function() {

});
```

### 创建一个拷贝任务

```
var gulp = require('gulp');
var copy = require('gulp-file-copy');

gulp.task('copy', function() {
	gulp.src('./app')
	.pipe(copy('./dist'), {
		start: './app'
	});
});
```

### 执行任务

```
gulp copy
```

## API
### gulp.src(globs[, options])

### gulp.dest(path[, options])
### gulp.task(name[, deps], fn)
### gulp.watch(glob [, opts], tasks)
### gulp.watch(glob[, opts, cb])

## 常用插件
* gulp-ruby-sass
* gulp-uglify
* gulp-clean
* gulp-autoprefixer

## 参考
* https://github.com/gulpjs/gulp




