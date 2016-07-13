# 事件的触发顺序
在浏览器中，同一个 DOM 元素可以绑定多个事件，这些事件并不是同时触发的，它们的触发是有顺序的，并且和浏览器本身的实现有关，不同的浏览器事件的顺序不一样。

## PC 端浏览器

我们试图在浏览器中运行如下 JS，来测试一下事件触发的顺序：

```
<script>
window.onload = function(){
	var btn = document.getElementById("btn");
	btn.onfocus = function() {
		console.log("onfocus");
	};
	btn.onmouseover = function() {
		console.log("onmouseover");
	};
	btn.onmousedown = function() {
		console.log("onmousedown");
	};
	btn.onmouseout = function() {
		console.log("onmouseout");
	};
	btn.onmouseup = function() {
		console.log("onmouseup");
	};
	btn.onclick = function() {
		console.log("onclick");
	};
	btn.onblur = function() {
		console.log("onblur");
	};
};
</script>
```

在 Chrome 中执行顺序如下：

* onmouseover
* onmousedown
* onfocus
* onmouseup
* onclick
* onmouseout
* onblur

## Mobile 端浏览器
由于 Mobile 端增加了`touch`事件，这样我们需要把脚本做下修改，增加和 touch 相关的事件：

```
<script>
window.onload = function(){
	var btn = document.getElementById("btn");
	btn.onfocus = function() {
		console.log("onfocus");
	};
	btn.onmouseover = function() {
		console.log("onmouseover");
	};
	btn.ontouchstart = function() {
		console.log("ontouchstart");
	};
	btn.ontouchcancel = function() {
		console.log("ontouchcancel");
	};
	btn.ontouchend = function() {
		console.log("ontouchend");
	};
	btn.ontouchmove = function() {
		console.log("ontouchmove");
	};
	btn.onmousedown = function() {
		console.log("onmousedown");
	};
	btn.onmouseout = function() {
		console.log("onmouseout");
	};
	btn.onmouseup = function() {
		console.log("onmouseup");
	};
	btn.onclick = function() {
		console.log("onclick");
	};
	btn.onblur = function() {
		console.log("onblur");
	};
};
</script>
```

在 iOS 的 safari 中执行顺序如下：

* ontouchstart
* ontouchend
* onmouseover
* onmousedown
* onfocus
* onmouseup
* onclick
* onblur


