# 频繁触发事件的性能问题
我们经常遇到由于事件频繁的调用方法，因而导致频繁操作 DOM、加载资源等大量的计算，从而导致 UI 卡顿甚至是浏览器崩溃的现象。主要场景如下：

* 浏览器滚动时绑定了事件（onscroll）；
* 浏览器大小改变时绑定了事件（onresize）；
* 拖拽 DOM 元素时绑定了事件（onmousemove）；
* 输入文字时绑定了事件（onkeydown, onkeyup, onchange）；

常常我们采用防抖动和稀释的方式来解决这些性能问题，专业的话术叫做函数去抖（debounce）和函数节流（throttle）。

# 函数去抖 debounce
所谓函数去抖，就是当调用函数 n 毫秒后，才会执行该函数，如果这 n 毫秒内又调用此函数则开始重新计算执行事件。
## 应用场景
* 浏览器滚动时绑定了事件（onscroll）；
* 浏览器大小改变时绑定了事件（onresize）；
## 简单实现
```
var debounce = function(fn, delay) {
    
}
```

# 函数节流 throttle
所谓函数节流，就是预先设定一个函数执行的周期 t 毫秒，当频繁执行函数时，当调用函数的时刻大于或等于 t 毫秒则执行并且开始进行下一个周期。
## 应用场景
## 简单实现
```
var throttle = function(fn, delay) {
    
}
```

# 参考
* http://drupalmotion.com/article/debounce-and-throttle-visual-explanation
* http://www.alloyteam.com/2012/11/javascript-throttle/
* http://benalman.com/projects/jquery-throttle-debounce-plugin/