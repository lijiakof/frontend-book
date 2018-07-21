# CSS3 `clip-path` & `clip-path` 打破矩形设计的限制
CSS 形状模块标准1（CSS Shapes Module Level 1）这个规范打破了 WEB 中的矩形盒模型的限制，并且将网页设计提升到一个新的高度。

## 关于 Shapes 规范 `shape-outside`
`shape-outside` CSS 属性可以来设置行内内容应该包裹的形状，默认形状为：`margin-box`。这一属性足以让我们发挥各种想象，从此我们的元素设计将不会仅仅存在于一个矩形内，它可以是任何形状。

* `shape-outside: none` ：没有形状就是默认形状：`margin-box`
* `shape-outside: margin-box` ：定义一个由外边距的外边缘封闭形成的形状。
* `shape-outside: border-box` ：定义一个由边界的外边缘封闭形成的形状。
* `shape-outside: padding-box` ：定义一个由内边距的外边缘封闭形成的形状。
* `shape-outside: content-box` ：定义一个由内容区域的外边缘封闭形成的形状

以上属性定义了整个区域的范围，其实和 `box-sizing` 比较类似。

* `shape-outside: circle()` ：定义一个圆形函数来绘制包裹形状。
* `shape-outside: ellipse()` ：定义一个椭圆形函数来绘制包裹形状。
* `shape-outside: inset()` ：
* `shape-outside: polygon()` ：定义一个多边形函数来绘制包裹形状。

以上属性是来绘制整个包裹区域的形状的，有点类似于 SVG 或者 Canvas 的绘制图像函数。

* `shape-outside: url()` ：定义区域内背景图。
* `shape-outside: linear-gradient()` ：定义区域内背景渐变色。

以上属性是来给整个区域设置背景的。

## 剪裁 `clip-path`
`clip-path` CSS 属性可以用来设置一个元素的剪切区域，区域内的部分显示，区域外的部分隐藏。

* `clip-path: none` 
* `clip-path: fill-box` 
* `clip-path: border-box`
* `clip-path: padding-box`
* `clip-path: content-box`

它们和 `shape-outside` 的区域范围定义比较类似。

* `clip-path: circle()`
* `clip-path: ellipse()`
* `clip-path: inset()` 

它们和 `shape-outside` 的绘制形状比较类似。

## 开始尝试
有了 `shape-outside` 和 `clip-path` 我们不仅可以让行内元素产生不同的形状（影响周边元素），而且可以让块级元素剪切成自己想要的形状。

以下 Demo 可以在这里修改查看：https://codepen.io/anon/pen/oMBwVK

```
<!DOCTYPE html>
<html>
<head>
<style>
    .circle-words {
        shape-outside: content-box circle();
        width: 200px;
        height: 200px;
        border-radius: 50%;
        background-color: #FF5A5F;
        float: left;
    }
    .circle-clip {
        background-color: #FF5A5F;
        width: 400px;
        height: 200px;
        clip-path: inset(15% 0 15% 0 round 0 15% 0 15%);
        margin: 30px;
        background-image: url(https://metaimg.baichanghui.com/METADATA/79d9a111-a39b-4904-b7e3-94068927d822);
    }
</style>
</head>
<body>
    <div style="width: 600px; margin: auto">
        <div class="circle-words"></div>
        Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World! Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World! Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World! Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!
        Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World! Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World! Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World! Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!Hello World!
    </div>
    <div style="width: 600px; height: 600px; margin: auto; background-color: #999">
        <div class="circle-clip">

        </div>
    </div>
</body>
</html>
```



## 参考
* https://www.sitepoint.com/css-shapes-breaking-rectangular-design/