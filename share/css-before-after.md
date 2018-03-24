# 巧用 `::before` `::after` 伪类
我们知道 `::before` 和 `::after` 是 CSS 中的伪类，它们基本功能是在 CSS 渲染中向元素的内容前面和后面插入内容。虽然在实际 HTML 中我们没有增加任何标签，但是它会帮我们渲染出来，并且会在浏览器中展现出来。

他们在实际的开发过程中我们使用的比较少，但是它的确有很巧妙的地方值得我们来开发：

## 动态在元素中添加字符串
* 场景：当需要在列表、或者多处相同样式中增加一个不变的字符串时，我们可以使用它；
* 例如：产品列表，价格的展示前面都加上 `¥`；
* 建议：注意场景，少使用；
* 代码：

```
.price::before {
    content: "¥"
}
```

## 不改变元素尺寸的边框
* 场景：在宽度为百分比的元素中，为此元素增加边框，此时会导致元素超过原有的百分比；
* 例如：导航条宽度为文档的 100% ，刚好撑满文档，但是需要在周围增加 1px 的边框，导致导航条宽度超过了浏览器宽度；
* 建议：中等程度使用，有替代方案；
* 代码：

```
.meun {
    width: 100%;
    height: 80px;
    position: relative;
}
.menu::before {
    content: ""
    position: absolute;
    left: 0;
    border-left: 1px solid #ccc;
}
.menu::after {
    content: ""
    position: absolute;
    left: 0;
    border-left: 1px solid #ccc;
}
```

## 0.5px 细边框
参考：[如何制作 0.5 像素的细线](share/css-half-border.md)

## 简单几何图形
* 场景：很多简单的几何图形，我们可以通过它们只需要用一个样式就可以解决问题；
* 例如：带尖角的气泡对话框、筛选组件的下拉箭头；
* 建议：强烈推荐；
* 代码：

```
.select::after {
    content: "";
    position: absolute;
    top: 50%;
    margin-top: -7px;
    right: 10px;
    display: inline-block;   
    border-left: 1px solid #000; 
    border-bottom: 1px solid #000;  
    width: 14px; 
    height: 14px;  
    transform: rotate(-45deg);  
}
```

## 清除浮动
* 场景：当一个元素在众多设置了浮动样式`float: left`的后面，但是又要另起一行时；
* 建议：强烈推荐；
* 代码：

```
.container::before {
    content: "";
    display: table;
} 

.container::after {
    clear: both;
}
```

## 总结
利用 `::before` `::after` 伪类，动态的在元素开始和末尾增加标签这一特性，我们可以做出很多丰富的样式但是有减少了 DOM 的复杂度，当然它还有更多更丰富的用法等待着我们来挖掘。
