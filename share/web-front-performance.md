# WEB 前端性能优化实战
网站的速度是关系用户体验的最重要因素之一，谁都会愿意得到快速的反馈，等待是一件让人烦躁的事情。所以性能优化对于网站来说是非常重要的的事情，它带来的好处不用解释。

## 跑车和轿车之间的较量
问题：如果跑车是轿车速度的两倍，是跑车快还是轿车快？

* 比赛1：从起点到终点的比赛：毫无疑问跑车肯定先到达终点，秒杀轿车；
* 比赛2：将 4 个人从起点运到终点：跑车此时就比较尴尬，因为只能带 1 位乘客，而轿车可以一次带 4 位乘客，显然跑车要来回好几趟，而轿车只需要 1 趟，明显轿车的效率更高。

所以性能的优化有两种：一种是快，体现在战术上，通过极致的优化算法达到目的；一种是高效，体现在战略上，通过合理的安排资源达到目的。快就是竞技赛强者胜；而高效是团体赛谋者胜；接下来我们会通过这两个方面对 WEB 前端做一次性能优化之旅。

## 浏览器请求整个页面的过程
前端性能优化，首先要搞清楚浏览器页面请求的整个过程，这样我们才能很好的解决问题。

![性能](../resources/images/h5-performance.png)

* navigationStart：准备加载新页面；
* unloadEventStart：开始卸载前一个页面；
* unloadEventEnd：页面卸载完成；
* redirectStart：开始重定型；
* redirectEnd：重定向完成；
* fetchStart：开始获取页面；
* domainLookupStart：开始 DNS 解析；
* domainLookupEnd：DNS 解析完成；
* connectStart：开始建立连接；
* connectEnd：建立连接完成；
* secureConnectionStart：开始建立 SSL 连接；
* requestStart：开始请求页面文档；
* responseStart：开始返回页面文档；
* responseEnd：返回页面文档完成；
* domLoading：当前页面开始解析；（domReady 白屏时间）
* domInteractive：页面解析完成，还没有开始加载资源（样式、脚本、图片）；
* domContentLoadedEventStart：页面解析完成，开始加载资源（样式、脚本、图片），开始运行脚本（同步的脚本）；
* domContentLoadedEventEnd：脚本运行完成（同步的脚本下载完成，首屏时间）
* domComplete：资源（图片等）加载完成
* loadEventStart：触发 load 事件的开始时间（onload）
* loadEventEnd：触发 load 事件的结束时间

## WEB 性能指标

* 白屏时间（domLoading）：
    * HTML
* 首屏时间（domContentLoadedEventEnd）：
    * 样式
    * 脚本（核心脚本）
* 页面加载完成（domComplete）：
    * 图片
    * 样式
* 所有资源加载完成（loadEventStart）：
    * 样式
    * 脚本（最底部脚本，通常是统计脚本）
    * 图片
    * iFrame（我们没有）

## 性能优化准则
前人已经给我们留下了很多 WEB 性能优化的准则，我们一一列举出来（来自雅虎 YSlow 团队对 Web 性能的总结）：

* [术]减重传输
    * 使用小 favicon.ico 文件
    * 减少 Cookie 大小
    * 静态资源文件使用无 Cookie 的域
    * 精简 JavaScript 和 CSS
    * 移除重复的脚本
    * 使用 Gzip 压缩
* [略]打包传输
    * 尽可能减少 HTTP 请求次数，合并 JavaScript 和 CSS
    * IconFont
* [略]合理的加载顺序
    * 在 HTML 文件顶部放置样式表
    * 在 HTML 文件底部放置 JavaScript 脚本
    * 图片按需加载
* [术]利用缓存
    * 使用外部 JavaScript 和 CSS 外部文件
    * 加入 Expires 或 Cache-Control Header
    * 配置 ETag
    * 缓存 AJAX
    * 使用 GET 完成 AJAX 请求
* [术]网络层
    * 使用 CDN

## 我们已经完成的优化
我们的 WEB 框架本身就完成了一些性能上的优化：

* 构建工具
    * 压缩合并静态文件
    * 生成小的 favicon.ico 文件
    * 合理的动态引用 JavaScript
    * 样式在前面，JavaScript 在后面
    * 自动拆分静态资源成为外部资源
* Node & Nginx
    * Gzip
    * Expires 和 Cache-Control Header
    * ETag

## 问题在哪儿？
为了看清楚慢到底在哪儿，通过一个浏览器来调试这种方式是不科学的，我们需要更多用户的数据分析数据，我们在网站中植入了性能收集的代码（听云），每个用户的性能都可以统计到，这样才能找到真正慢在哪儿。

我们分析了所有慢的网页，发现了网站慢的普片原因：图片加载太慢！



## 解决方案

* 站内图片
    * 保证不影响视觉的情况下，压缩大小
    * 小图片 Base64
    * icon 采用 iconfont 将图标文件一次性输出
* 站外图片
    * 采用按需加载
    * 分享二维码 
* 组件中的图片
    * 整个组件按需加载
* 站外脚本
	* ABTesting
* 快速显示 Dom

## 优化完的结果
优化完了我们不能说就结束了，我们需要看最终结果是不是好的，是否是真的达到了我们的目的，通过数据可以验证我们的方案是否成功，如果不成功到底错在哪里，我们需要如何改进。











