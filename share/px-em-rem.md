# px、em、rem
`px`、`em`和`rem`都是网页设计中的长度单位。`px`是最常用的单位，它是 Pixel 的缩写代表像素，它是计算机图像显示的最小单位；`em`（font size of the element）它是一个相对大小的单位，它的大小会相对于它的父级元素的字体大小；`rem`（font size of the root element）它也是一个相对大小的单位，但是它相对于根元素字体的大小；

## px
`px`单位就不用多解释了，非常常见。

## em
`em`是为了解决长度单位在不同缩放比例下浏览器中能够更好的显示的问题，它是一个相对大小的单位，但是它是相对于其父级元素的大小，这样导致了一个问题，在现在复杂的网页下，层层嵌套的元素最终导致很难控制大小。

```
html {
	font-size: 62.5%; /*10 ÷ 16 × 100% = 62.5%*/
}

body {
	font-size: 1.2em; /*1.2em × 10 = 12px */
}

p {
	font-size: 1.2em; /*1.2em × ? = ? */
}
```

上面的例子我们可以看到，浏览器默认字体大小为`16px`，`html`的字体大小为`16px * 62.5% = 10px`；`body`的字体大小相对于`html`元素，计算出来为`12px`；但是`p`的大小很难计算，因为我们不知道它的父级元素的字体大小到底是多少，它的父级元素字体大小可能是`12px`、也有可能是`20px`，它太难控制了。

## rem
CSS3 的出现让我们对相对的尺寸有了更加稳定的设置方法：那就是`rem`。`rem`它是相对于根元素来计算大小的：

```
html {
	font-size: 62.5%; /*10 ÷ 16 × 100% = 62.5%*/
}

body {
	font-size: 1.2rem; /*1.2rem × 10 = 12px */
}

p {
	font-size: 1.4rem; /*1.4rem × 10 = 14px */
}
```

这样一来，我们可以很好的来控制长度单位的大小，它是一旦设置就是一个稳定的值；我们经常在响应式设计中采用这样的长度单位，通过媒体查询可以让长度单位更加符合不同屏幕大小：

```
html {
    font-size: 62.5%; /* 10 ÷ 16 × 100% = 62.5% */
}

@media screen and (min-width: 400px) {
    html {
        font-size: 75%; /* 12 ÷ 16 × 100% = 75% */
    }
}

@media screen and (max-width: 319px) {
    html {
        font-size: 50%; /* 8 ÷ 16 × 100% = 50% */
    }
}
```

## 参考

* [CSS Font-Size: em vs. px vs. pt vs. percent](http://kyleschaeffer.com/development/css-font-size-em-vs-px-vs-pt-vs/)；
* [Using Points, Pixels, Ems, or Percentages for CSS Fonts](http://webdesign.about.com/cs/typemeasurements/a/aa042803a.htm)

