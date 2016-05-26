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

### 创建一个 js 压缩混淆任务

```
var gulp = require('gulp');
var uglify = require('gulp-uglify');

gulp.task('uglify', function() {
	return gulp.src('app/scripts/app.js')
	.pipe(uglify())
	.pipe(gulp.dest('debug/scripts/'));
});
```

### 执行任务

```
gulp uglify
```

## API
### gulp.src(globs[, options])
通过`glob`模式来设置需要处理的文件。它会返回一个 `Vinyl files`（vinyl 文件对象）的 `stream`（数据流），它可以被 piped 到插件中去。

* globs：需要处理的文件；
* options：
	* options.buffer：
	* options.read：
	* options.base：

[glob](https://github.com/isaacs/node-glob) 是通过特殊的匹配字符串，返回文件和文件夹。[vinyl-fs](https://github.com/wearefractal/vinyl-fs) 是一种“虚拟文件格式”。

### gulp.dest(path[, options])
这个方法创建了一个可写流，它重新使用可读流中的文件名，然后在必要时创建文件夹。在写入操作完成后，你能够继续使用这个流（比如：你需要使用gzip压缩数据并写入到其他文件）。

* path：输出路径；
* options：

### gulp.task(name[, deps], fn)
定义一个任务。

* name：任务名称；
* deps：任务依赖列表，列表中的任务会在当前任务执行之前完成；
* fn：该函数定义任务所要执行的一些操作，通常形式：`gulp.src().pipe(plugin()).pipe(gulp.desc())`

### gulp.watch(glob [, opts], tasks)
它是用来监视文件，当文件发生改变后会执行一些任务。

* glob：监控哪些文件的变动；
* opts
* tasks：执行哪些任务；

### gulp.watch(glob[, opts, cb])
它是用来监视文件，当文件发生改变后会执行回调。

* glob：监控哪些文件的变动；
* opts
* cb：回调函数；

## 常用插件
* gulp-ruby-sass
* gulp-uglify
* gulp-clean
* gulp-autoprefixer

## 参考
* https://github.com/gulpjs/gulp
* http://gulpjs.com/plugins/




