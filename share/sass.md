# 认识 SASS
## 什么是 SASS
SASS 是对 CSS 的扩展，是 CSS 的预编译语言，让 CSS 如虎添翼。它让你在编写 CSS 时可以使用变量、嵌套、mixins、导入等功能，让 CSS 更佳程序化和模块化。
## 语法
### import
SASS 可以通过`@import`来导入其它的 SASS 文件，编译时会将导入的 CASS 文件合并并且生成一个 CSS 文件。导入的文件名可以忽略后缀`.scss`，例如：导入文件`index.scss`，只需要写成：`@import index`就行了。

```
//input:SASS
//index.scss
.index {
	color: #333;
}
//app.scss
@import "index";
body {
	color: #ccc;
}
```

```
//output:CSS
//app.css
.index {
	color: #333;
}
body {
	color: #ccc;
}
```

### 注释
SASS 支持两种方式的注释，一种多行注释：`/*注释内容*/`，另一种单行注释：`//注释内容`
### 变量
SASS 的变量都以 $ 开头，后面紧跟变量名，变量名和变量值之间需要用`:`分开，如果值后面加上`!default`表示默认值。
#### 普通变量：

```
//input:SASS
$color: #ccc;
body {
	color: $color;
}
```

```
//output:CSS
body {
	color: #ccc;
}
```

#### 默认变量
默认变量只需要在值后面加上`!default`

```
//input:SASS
$color: #ccc !default;
body {
	color: $color;
}
```
#### 特殊变量
一般我们定义的变量都为属性值，单如果变量时属性的某一部分，我们以`#{$variables}`形势来使用。

```
//input:SASS
$direction: top;
body {
	margin-#{$direction}: 20px;
}
```

```
//output:CSS
body {
	margin-top: 20px;
}
```
#### 多值变量
我们可以像 JS 中使用数组活动对象的形势来定义多值变量
* list 数据通过空格、逗号或者小括号分割多个值，可以用`nth($var, $index)`来取值，注意`$index`以 1 开始。

```
//input:SASS
$var: 12px #ccc #ddd;
body {
	font-size: nth($var, 1);
	color: nth($var, 2);
	background-color: nth($var, 3);
}
```

```
//output:CSS
body {
	font-size: 12px;
	color: #ccc;
	background-color: #ddd;
}
```

* map 数据可以以 key 和 value 成对出现，格式为：`$map:(key: value, key1: value1)`可以通过`map-get($map, $key)`来取值，

```
//input:SASS
$heads: (h1: 20px, h2: 18px, h3: 16px);
@each $h, $size in $heads {
	#($h) {
		font-size: $size;
	}
}
```

```
//output:CSS
h1 {
  font-size: 20px; 
}
h2 {
  font-size: 18px; 
}
h3 {
  font-size: 16px; 
}
```

### 嵌套
SASS 嵌套有两种：一种选择器的嵌套，一种是属性的嵌套，当然我们使用选择器的嵌套更多一些。
#### 选择器嵌套
所谓选择器的嵌套，指的是在一个选择器中嵌套另一个选择器来实现继承，从而增强了 SASS 文件的结构和可读性。可以使用`&`表示父级元素选择器。

```
//input:SASS
.index-page {
	nav{
		font-size: 14px;
	}
	article{
		font-size: 12px;
	}
	footer {
		font-size: 14px;
	}
}
```

```
//output:CSS
.index-page nav{
	font-size: 14px;
}
.index-page article{
	font-size: 12px;
}
.index-page footer{
	font-size: 14px;
}
```
#### 属性嵌套

```
//input:SASS
.index-page {
	nav{
		border: {
			style: solid;
			left: {
				width: 1px;
				color: #333;
			}
		}
	}
}
```

```
//output:CSS
.index-page nav{
	border-style: solid;
	border-left-width: 1px;
	border-left-color: #333;
}
```

### mixin
SASS 中用`@mixin`声明混合，类似于方法可以复用、传参，参数名用`$`开始，多个参数用`,`分开，也可以给参数设置默认值。通过`@include`来调用声明的`@mixin`

```
//input:SASS
@mixin borderTop ($borderStyle:1px #fff solid) {
    position: relative;
    &:before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 200%;
        -webkit-transform: scale(0.5) translate(-50%, -50%);
        transform: scale(0.5) translate(-50%, -50%);
        border-top: $borderStyle;
    }
}

.list {
	color: #ccc;
	@include borderTop(1px #ccc soild);
} 
```

```
//output:CSS
.list{
	color: #ccc;
	position: relative;
	&:before {
		content: "";
		position: absolute;
		top: 0;
		left: 0;
		width: 200%;
		-webkit-transform: scale(0.5) translate(-50%, -50%);
		transform: scale(0.5) translate(-50%, -50%);
		border-top: 1px #ccc soild;
	}
}
```

### 继承
SASS 可以通过`@extend`来继承另一个选择器的所有样式：

```
//input:SASS
.ellipsis {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
.title {
	width: 100px;
	@extend .ellipsis;
}
```

```
//output:CSS
.title {
	width: 100px;
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
}
```

### 函数
### 运算
### 条件判断及循环

