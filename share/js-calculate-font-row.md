# 如何计算文本的行数
在前端经常会遇到内容太多了，需要对多余的内容进行截取并打上省略号的问题。CSS2 可以解决超出一行省略的问题，Chrome 可以通过`-webkit-line-clamp`来实现多行省略的问题，还是还需要配合其它的一些样式属性。解决这个问题的主要问题在于**如何计算当前 DOM 内部的文本的行数**。

## 方案
要想知道文本的行数，那就需要知道文本的总高度和每一行的高度，总高度除以行高就是行数。当然总高度的计算必须是文字所在的 DOM 没有对高度的限制，随着文本的增加 DOM 要随之变高才行；最后还要考虑 DOM 的样式`padding`和`margin`对高度的影响。这样一来我们就可以计算出文本的行数了。

### 清除文本在 DOM 内部高度的限制
拷贝文本所在的 DOM，将 DOM 的`width`、`padding-right`、`padding-left`、`margin-right`、`margin-left`保持和原有 DOM 一致；清除文本的`height`、`padding-top`、`padding-bottom`、`margin-top`、`margin-bottom`样式，这样一来文本就处于一个没有高度限制的 DOM 中，而且高度不受`padding`和`margin`的影响，并且它的宽度和原有宽度保持一致。

**注意：代码基于 zepto.js**

```
var getRow = function(id) {
	var clone = $(id).clone().appendTo('body');

	// clear some style
	clone.css({
		"height":"auto",
		"padding-top": 0,
		"padding-bottom": 0,
		"margin-top": 0,
		"margin-bottom": 0
	});
	
	// todo...
};
```

### 获取行高
获取行高比较容易，直接通过`window.getComputedStyle(element, null)`可以获取元素所有的最终所使用的样式。

```
var getRow = function(id) {
	var clone = $(id).clone().appendTo('body');

	// clear some style
	// ...
	
	// get line-height
	var style = window.getComputedStyle(clone[0], null);
	var fontSize = style.fontSize;
	var lineHeight = style.lineHeight === "normal" ? fontSize : style.lineHeight;
	
	// todo...
};
```

### 计算行数
计算行数前必须要知道总高度，总高度获取的方式太多了，可以和获取行高一样通过`window.getComputedStyle(element, null)`，也可以通过`document.clientHeight`获取，当然像 jQuery 等框架已经把获取文档对象的高度封装好了。

**注意：在计算之前需要把`px`转成数字类型**

```
var pxToNumber = function(px) {
   var num = Number(px.replace("px", ""));

   return num;
};

var getRow = function(id) {
	var clone = $(id).clone().appendTo('body');

	// clear some style
	// ...
	
	// get line-height
	// ...

	//get row count
	var height = style.height;
	var row = pxToNumber(height) / pxToNumber(lineHeight);
	
	return row;
};
```

### 最后
当然我们在计算高度的时候，这个克隆出来的 DOM 不能让用户看到，可以通过`opacity: 0.0001`或者`visibility: hidden`来隐藏它。

```
var pxToNumber = function(px) {
   var num = Number(px.replace("px", ""));

   return num;
};
var getRow = function(id) {
	var clone = $(id).clone().appendTo('body');

	// clear some style
	clone.css({
		"height":"auto",
		"padding-top": 0,
		"padding-bottom": 0,
		"margin-top": 0,
		"margin-bottom": 0,
		"visibility": "hidden"
	});
	
	// get line-height
	var style = window.getComputedStyle(clone[0], null);
	var fontSize = style.fontSize;
	var lineHeight = style.lineHeight === "normal" ? fontSize : style.lineHeight;

	//get row count
	var height = style.height;
	var row = pxToNumber(height) / pxToNumber(lineHeight);
	
	return row;
};
```


