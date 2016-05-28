# Mobile Web 如何制作 0.5 像素的细线
移动技术的出现，将屏幕显示技术带入 Retina 级别，现在绝大部分移动端设备都达到或者超过了 Retina 级别，但是由于移动端的屏幕分辨率和浏览器的分辨率有一些换算关系，导致在浏览器中的 1 个像素会占用屏幕的 2 个或者 2 个以上的像素，这样使得 Mobile Web 的 UI 在精细程度上大打折扣。但是我们还是有技术的手段来解决这个问题。

## border-width: 0.5px
直接通过样式来设置`0.5px`的边框。当然这个方案是非常理想的方案，但是事实总是残酷的，它只在 iOS 8+ 上支持，对于 Android 无法支持。不过个人相信时间会让它成为最佳可行方案。

```
.border {
	border: 1px #000 solid;
}
@media(min-device-pixel-ratio: 2) {
	border-width: 0.5px;
}
```

当然对于这种兼容性问题我们可以通过 JavaScript 来做降级处理，我们将默认边框设置为`1px`，分辨出来是 iOS 并且版本为 8 以上，就可以将边框设置为`0.5px`；

缺点:

* 兼容性问题。

## 利用 box-shadow
我们也可以利用 box-shadow 的阴影来绘制出`0.5px`的线，只需要将阴影的偏移半径设置的足够小，模糊半径设置为`1px`。这种比较难控制，颜色很难取，并且它并不是一条真是的线。

```
.border {
	box-shadow: 0 -1px 1px -1px rgba(0, 0, 0, .5),
					-1px 0 1px -1px rgba(0, 0, 0, .5),
					1px 0 1px -1px rgba(0, 0, 0, .5),
					0 1px 1px -1px rgba(0, 0, 0, .5);
}
```
缺点：

* 仔细观察它并不是一条实线；
* 由于是阴影，要取到想要的颜色很难。

## border-image
其实就是利用图片的缩放来解决这个问题，需要一张 6*6px 的图片，做成特殊的透明框线图；但是它不支持圆角。

```
.border {
	border-width: 1px;
	border-image: url(border.png) 2 repeat;
}
```
缺点：

* 需要额外的图片；
* 不支持圆角边框；
* 颜色固定，无法随意设置。

## viewport
利用动态的改变`<viewport>`让网页的像素区域和屏幕的像素区域重合，这样就很轻松的去写`1px`的边框，并且它会等于屏幕本身的`1px`。

```
//当 devicePixelRatio = 2 输出：
<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">

//当 devicePixelRatio = 3 输出：
<meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no">

```
缺点：

* Mobile APP 在设计上会存在问题（我们会在专门的 viewport 中去讲解）

## 利用伪类的缩放
利用 ::before 或 ::after 画出是`DOM`的尺寸 2 倍或 3 倍的绝对定位伪对象，使用 1px 的 border 定义新边框后，让后通过设置 transform 的 scale 把伪类对象缩小到一半或 1/3，这样看上去伪对象就和容器一样大了。

```
.border {
    border-bottom: 1px solid #ccc;
}
@media (min-device-pixel-ratio: 2) {
    .border {
        position: relative;
        border-bottom: none;
    }
    .border::after {
        content: ' ';
        display: block;
        position: absolute;
        left: 0;
        bottom: 0;
        width: 200%;
        height: 0;
        border-bottom: 1px solid #ccc;
        transform: scale(0.5) translate(-50%, -50%);
    }
}
```
缺点：

* 占用伪类；
* 容器的定位被设置了；
* 代码太复杂。

## 总结
这些方案中各有利弊，需要结合实际项目情况来做，当然我们相信随着时间的推移，各大浏览器厂商会支持`border-width: 0.5px`这一样式。

## 参考
* https://51bits.com/writing/half-point-css-borders-in-ios/


