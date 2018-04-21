# 开启 CSS 硬件加速，提高网站性能
随着互联网和电子硬件设备的不断发展，WEB 应用的交互也越来越复杂（代码层面），为了得到最佳体验 WEB 应用开始使用更多的动画来做顺畅的交互，但这也给浏览器增加了额外的负担。庆幸的是现在大多数计算机设备不仅有强大的 CPU 而且有了专门为图形计算而生的 GPU，我们可以充分发挥 GPU 的实力，来让我们的 WEB 应用更佳的流畅。

## 开启 CSS 硬件加速
CSS 动画 `transitions` 和 `transforms`，并不会自动的使用 GPU 来处理，而是通过浏览器软件的渲染引擎来处理的。那么到底如何做到它们来通过 GPU 来处理呢？很多浏览器通过特定的 CSS 规则提供 GPU 加速。

目前，很多浏览器包括：Chrome、FireFox、Safari、IE9+ 以及最新版本的 Opera 都支持硬件加速；他们只会被使用在对 DOM 元素有益处的地方，例如：当 3D 变换的样式出现时会使用 GPU 加速：

```
.cube {
    -webkit-transform: translate3d(250px, 250px, 250px)
    rotate3d(250px, 250px, 250px, -120deg)
    scale3d(0.5, 0.5, 0.5);
}
```

### 采用 `transform: translateZ(0)`
在某种情况下，你可能不希望元素上进行 3D 变换，但是仍然可以利用 GPU 加速，这个时候我们可以通过一些简单的 CSS 属性来触发浏览器的 GPU 加速。即使是我们不采用 3D 动画也是开始使用 3D 渲染的。采用 `transform: translateZ(0)` 可以在现代桌面和移动端浏览器触发 GPU 加速：

```
.cube {
    -webkit-transform: translateZ(0);
    -moz-transform: translateZ(0);
    -ms-transform: translateZ(0);
    -o-transform: translateZ(0);
    transform: translateZ(0);
}
```

**`transform: translateZ(0);` 表示在浏览器的 Z 轴上移动。**

在 Chrome 和 Safari 中，当使用 CSS 变化或动画时，我们可能会看到闪烁的效果，我们可以通过下面的 CSS 代码来解决：

```
.cube {
    /*隐藏被旋转的背面*/
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    -ms-backface-visibility: hidden;
    backface-visibility: hidden;

    /*定义 3D 元素距视图的距离*/
    -webkit-perspective: 1000;
    -moz-perspective: 1000;
    -ms-perspective: 1000;
    perspective: 1000;
}
```

### 采用 `transform: translate3d(0, 0, 0)`
另外一个不错的方法就是利用 CSS `transform: translate3d(0, 0, 0)` 来启动 GPU 加速：

```
.cube {
    -webkit-transform: translate3d(0, 0, 0);
    -moz-transform: translate3d(0, 0, 0);
    -ms-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
}
```

## 总结
Native 移动端应用可以可以很好的利用设备的 GPU，这就是为什么 Native 应用会比 Web 应用的性能好一些的原因。但是我们并不能一味的追求这种硬件加速而全部采用，这并不是一个明智的选择，你只能在真正需要使用性能加速的地方去使用，不必要的 GPU 加速会增加内存的使用，让你的移动设备的电池寿命有所影响，所以我们在真正的产品中使用时要能够辨识哪些需要硬件加速，哪些不需要，不要滥用。


## 参考
* http://blog.teamtreehouse.com/increase-your-sites-performance-with-hardware-accelerated-css
* https://aerotwist.com/blog/on-translate3d-and-layer-creation-hacks/