# 如何垂直居中
在 CSS 布局中，我们经常会遇到一个问题，让某个 DOM 垂直居中在它的父级元素中。这个问题需要分一些情况，在不同的情况下采用的方案不一样。

## 采用 `line-height` 属性
这种方式很常见，当 `line-height` 和 `height` 两个属性设置相同的高度时，该元素内部的文字将会居中。

```
#parent {
	height: 100px;
	line-height: 100px;
	border: solid 1px #333;
}
```

优缺点：

* [优点]设置简单；
* [缺点]只能对一行文字进行垂直居中；

## 采用 `display:table-cell` 和 `vertical-align:middle`
这种现实方式可以让标签元素以表格单元格的形式呈现，标签就像 table 中的 td，这样一来我们就可以通过`vertical-align:middle`这个样式使得其内部的元素居中显示。

```
#parent {
	height: 100px;
	display: table-cell;
	vertical-align: middle;
	border: solid 1px #333;
}
```

优缺点：

* [优点]设置多行文字居中；
* [缺点]会被其它样式破坏例如：`float`、`position:absolute`；


## 采用 `position: absolute` 和 `margin-top`
通过绝对定位可以给元素设置距父元素上部`top:50%`，但是还没结束该元素还需要做一定的偏移才行，偏移量为该元素的一半高度`margint-top:-height/2`。

```
#parent {
	height: 100px;
	position: relative;
	border: solid 1px #333;
}

#child {
	height: 20px;
	margin-top: -10px;
	position: absolute;
	top: 50%;
}
```

优缺点：

* [优点]居中元素对其它同级元素没有影响；
* [缺点]子元素的高度需要固定；

## 采用 `padding-top` 和 `padding-bottom`
这种方式只需要将顶部和底部的padding设置同样高度就行。

```
#parent {
	padding-top: 20px;
	padding-bottom: 20px;
	border: solid 1px #333;
}
```

优缺点：

* [优点]父级元素高度可变；
* [缺点]父级元素高度可变；

## 总结
垂直居中的方式很多，都有优缺点，我们需要根据不同的需求和布局来确定使用哪种方案。

