# 响应式网站实战
随着互联网技术的发展，网络终端越来越丰富，从最早的桌面设备到现在丰富的桌面+平板+手机端，为了适应各种不同的屏幕，但是在各个端都开发各自不同的站点会让开发资源大大的浪费，可复用性及差，这样一来响应式的网站设计应用而生。那么如何才能做到响应式，如何才能让同样的代码在不用设备上都有更好的交互体验呢？让我们看看具体如何实践：

* 屏幕尺寸定义
* 媒体查询
* 栅格系统
* 响应式字体
* 响应式尺寸
* 响应式组件

## 屏幕尺寸定义
首先，我们要定义好什么样的尺寸是桌面端、什么样的尺寸是平板、什么样的尺寸是手机端，我们需要参考大量的数据来做一个权衡的定义，结果如下表：

| 设备 | 屏幕大小 | 描述 |
| - | :-: | -: |
| 手机 | < 568px |  | 
| 大手机 | ≥ 568px |  | 
| 平板 | ≥ 768px |  | 
| 桌面 | ≥ 1024px |  | 
| 大桌面 | ≥ 1280px |  |

* 参考：https://material.io/devices/ *

这样一来，我们可以通过在不同的尺寸下设置不同的样式来达到响应式的效果，但是这种变化是需要有一定的规则的，不然在管理和维护上是灾难。

## 媒体查询
CSS3 加入了媒体查询功能，让样式可以识别不同的设备甚至是屏幕大小，有了这一功能后，让我们应对不同屏幕样式的问题上也变得简单起来。

以下是一个简单的媒体查询样式写法，它可以让 `<body>` 标签下的字体在屏幕宽度在 `568px` 以下的设备字体只有 `14px`，而超过 `568px` 的屏幕字体大小为 `16px`。由此一来，字体会随着屏幕的大小而变化：

```
body {
    font-size: 16px;
}

@media screen and (max-width: 568px) {
    body {
        font-size: 14px;
    }
}
```

更多媒体查询的相关知识可以查看：https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries

有了媒体查询后，我们可以在前面定义好的尺寸中编写适合屏幕大小的响应式 CSS。

| 关键词 | CSS 媒体查询 | 适用于 | Class |
| - | :-: | -: | -: |
| - | - | 所有 | .* | 
| xs | @media screen and (max-width: 35.4em) | < 568px | 暂无 | 
| sm | @media screen and (min-width: 35.5em) | ≥ 568px | .*-sm | 
| md | @media screen and (min-width: 48em) | ≥ 768px | .*-md | 
| lg | @media screen and (min-width: 64em) | ≥ 1024px | .*-lg | 
| xl | @media screen and (min-width: 80em) | ≥ 1280px | .*-xl |

## 栅格系统
栅格系统是解决响应式布局的一套样式系统，通常有这样几种类型的栅格系统：10 等份、12 等份、24 等份，实现方式主要有两种：百分比和 `flex`，在 CSS3 普及不大的年代通常采用百分比，现在绝大多数现代浏览器已经支持 `flex` 布局，推荐使用。

UI 必须在栅格系统中设计并符合栅格系统的规则，整体布局内的组件也需要符合栅格系统的规范。这样一来布局都会随着屏幕大小而调整，并且保证不同尺寸的最佳体验。

以下是 `pure` 框架的使用方式，其他响应式样式框架也差不多：

### 基本用法

```
<div class="pure-g">
    <div class="pure-u-1-3"> ... </div>
    <div class="pure-u-1-3"> ... </div>
    <div class="pure-u-1-3"> ... </div>
</div>
```

### 响应式栅格

```
<div class="pure-g">
    <div class="pure-u-1 pure-u-md-1-3"> ... </div>
    <div class="pure-u-1 pure-u-md-1-3"> ... </div>
    <div class="pure-u-1 pure-u-md-1-3"> ... </div>
</div>
```

### 有间隙的栅格

```
<div class="pure-g pure-g-span-1-24">
    <div class="pure-u-1 pure-u-md-1-3">
        <div class="pure-span-1-24"> ... </div>
    </div>
    <div class="pure-u-1 pure-u-md-1-3">
        <div class="pure-span-1-24"> ... </div>
    </div>
    <div class="pure-u-1 pure-u-md-1-3">
        <div class="pure-span-1-24"> ... </div>
    </div>
</div>
```

### 有偏移量的栅格

```
<div class="pure-g">
    <div class="pure-u-1 pure-u-md-1-3">
        <div class="offset-1-24"> ... </div>
    </div>
    <div class="pure-u-1 pure-u-md-1-3">
        <div class="offset-1-24"> ... </div>
    </div>
    <div class="pure-u-1 pure-u-md-1-3">
        <div class="offset-1-24"> ... </div>
    </div>
</div>
```

## 响应式字体
对于字体的解决方案，我们也有一套规则：

* px：字体在任何设备上大小都保持一致，此时用 px
    * 正文
    * 注释、解释
* rem：字体在不同设备上大小不一样时，采用 rem，rem 在每个不同的屏幕大小之间的换算关系参考下表
    * 标题
    * 

### rem 在不同设备下的对应关系表

| 关键词 | 屏幕大小 | rem = px |
| - | :-: | -: |
| xs | < 568px | 1rem = 8px | 
| sm | ≥ 568px | 1rem = 8px | 
| md | ≥ 768px | 1rem = 10px | 
| lg | ≥ 1024px | 1rem = 12px | 
| xl | ≥ 1280px | 1rem = 12px | 

## 响应式尺寸
同： `响应式字体`

## 响应式组件
当然对于非常特殊的组件，我们需要特殊对待，它们在桌面端和移动端相差太大，我们要采用组件的方式将复杂的逻辑封装进去，对外暴露相同的接口，让使用者在业务开发中不用考虑多个屏幕的适配问题：

* 图片浏览组件
* 对话框
* 导航
* 表单
* ...

## 响应式设计九大原则
以下是响应式设计所遵循的 9 大原则，我们会在以后的文章中一一介绍

* 响应式 vs 自适应网页设计
* 内容流动
* 相对单位
* 断点
* 最大值和最小值
* 嵌套对象
* Mobile 优先，还是 Desktop 优先
* 网页字体 vs 系统字体
* 位图 vs 矢量图

* 参考：http://blog.froont.com/9-basic-principles-of-responsive-web-design/ *

## 总结
响应式设计并不是一件非常困难的事情，需要我们定义好规则，让开发和设计都遵循这一规则，化繁为简，让开发更佳高效。