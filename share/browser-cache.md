# 浏览器缓存
浏览器为了提高自身的性能，它会有一些缓存，缓存的种类很多让我们一一了解：

## 基于 HTTP 协议的缓存
由于 HTML 是基于 HTTP 协议传输的，当然浏览器需要去实现这些缓存：

* Expires
* Cache-Control（max-age）
* Last-modified
* ETag

## Web SQL
本地数据库是 HTML5 提供的新的 API 接口，它可以通过 JavaScript 在本地创建一个数据库，将数据存储在本地，当然数据库的好处顾名思义可以通过一些类似 SQL 语句方便的对数据库进行操作。

## IndexDB
本地数据库的确是一个非常好的存储，不过 W3C 的规范中已经不在维护，取而代之的是 IndexDB，一种本地 NoSQL 类型的数据库，它也是互联网高速发展的产物，在性能和复杂逻辑的权衡。

## Local Storage
本地存储中的 LocalStorage 使用的很普遍，通常以 key/value 的方式存储，它会永久的存储在本地，需要主动清除。

## Session Storage
类似于 LocalStorage，与之不同的是它的存储有效期是 Session 级别，当关闭浏览器存储内容随之消失。

## Cookies
Cookies 是 Web 一直以来使用的存储，当然它的存储空间有限，通常只会存储一些标识符，加上它可以将数据发送到服务端，这样服务端可以通过这个标识符来存储更多的内容。

## Application Cache
离线缓存，通过 manifest 文件将静态资源文件按照 manifest 的要求自动的更新，让 WebApp 的更新更加的灵活。

## Cache Storage
cacheStorage 是用来存储 HTTP 请求中的 Response 对象的。当然 webStorage 可以实现这一种存储，但是 cacheStorage 会更加专业。

## 总结
浏览器有多种缓存，当然它们需要配合使用，那种数据适合什么样的存储。大家可以通过打开 Chrome 的调试工具的 Resources 中找到这些缓存。