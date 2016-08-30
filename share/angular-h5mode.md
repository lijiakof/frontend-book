# 如何开启 Angular 单页应用的 H5 路由模式
单页应用经常会采用 url 的 hash(锚点) 来实现路由机制，当然这种方式的确是一个有效的方式，但是在很多人会对 url 出现 hash 产生不好的感觉。H5 history 新 API 的出现，让浏览器地址栏的 url 改变而不刷新页面。

像 Angular 这样的框架，内部已经实现了 H5 模式的路由方式，只需要在 `config` 中设置 `$locationProvider.html5Mode(true)` 即可完成 url 变化而不会刷新页面。但是会存在一个问题，当路由到某一个页面时，由于本身没有这个页面，导致刷新浏览器去访问这个 url 时出现找不到该页面的错误。

当然，这种问题我们可以通过 Nginx 将 404 状态的路由都转发到首页，这样就解决了刷新找不到页面的问题。

## 解决步骤：

### 修改 Angular 应用的路由为 H5 模式：

```
var app = angular.module("app", [...]);
app.config(["$locationProvider", function($locationProvider){
	$locationProvider.html5Mode(true);
	...
}]);
```

### 在页面中增加 base url

```
<html>
	<head>
		<base href="/" />
	</head>
</html>
```

### 将 Angular 应用中的所有内部跳转改为 Angular 封装好的跳转：

* 页面上的链接跳转采用 `ng-ref`；
* 如果采用第三方 ui-router 的话，页面链接跳转采用：`ui-sref`；
* 模块内部的跳转采用 `$state.go`，例如在 `Controller` 中跳转；

### 配置 Nginx 将 404 状态的路由都转发到首页：

```
location / {
	root   /path;
	index  index.html index.htm;
	
	# 注意，‘ =404’ 前面必须有空格！
	try_files $uri $uri/ /index.html =404;
}
```

## 最后
这样一来，Angular 单页应用就可以去除 `#` 并且每个路由都可以直接进入正确的页面。再加上 Prerender 服务，会让网站的 SEO 更佳友好。

