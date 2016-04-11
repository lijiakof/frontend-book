# 跨域问题
我们在 Web 开发过程中经常使用 Ajax 来异步的获取数据，尤其是在前后端分离的架构中基本上都是通过 Ajax 获取数据，渲染页面都由前端浏览器来完成。通常这种前后端分离的架构，对于前端开发者来说已经像开发客户端 App 一样的开发 Web，获取数据也有专门封装好的 RESTful Client 组件来获取数据，不过它的原理也是使用了浏览器的 XMLHttpRequest 这个对象来实现异步请求数据。

但是当在域 domain-a.com 下想要通过 Ajax 来请求域 domain-b.com 的数据时，会遇到跨域的问题，通常这种跨域是不被浏览器允许的，如何解决呢？

## JSONP 
JSONP 是一个解决方案，它的原理：
* 在 HTML 中创建一个`<script src="">`标签，其中`src`属性中的地址就是接口地址；
* 并创建好**回调方法**，把得到数据后的逻辑写到这个方法中；
* 接口地址返回一段 JavaScript ，其内容为执行**回调方法**并传入 JSON 数据；
* 在创建`<script src="">`标签的同时监听`onload`或者`onreadystatechange`事件；
* 当这一段 JavaScript 下载完成后，立即运行该段 JavaScript；
* 此时已经拿到了数据，并开始执行处理这部分数据的逻辑。

当然这种方式巧妙的利用了外部的引用脚本来避开跨域，但是它只能 GET，不能进行 POST、PUT、DELETE 等 HTTP 方法，是一个阉割版的跨域方案。

## Flash
这种方式采用浏览器插件来解决跨域问题，不过 Flash 面对强大的 HTML5 逐渐消失在历史的舞台，这种方式不太建议使用。如果有兴趣的同学可以看相关文档[点这里](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html)。

## CORS
CORS 定义一种跨域访问的机制，可以让 Ajax 实现跨域访问。CORS 允许一个域上的网络应用向另一个域提交跨域 Ajax 请求。实现此功能非常简单，只需由服务器发送一个响应标头`Access-Control-Allow-Origin：*`即可。

## Proxy
代理的方式，通过代理将原本发送到另外一个域名的请求，发送到当前域下的某个 URL，通过服务器端转发到目标服务器，这样就不存在跨域问题了。


