# Prerender
搜索引擎经常试图来抓取我们的网站，但是搜索引擎不能执行 JavaScript 脚本，Prerender 服务就是来解决这一问题。Prerender 可以对这些使用了前端渲染的 JavaScript 框架做的网站进行良好的 SEO 优化。

基于 SEO 这样的场景，Prerender 是一个采用[phantomjs](http://phantomjs.org/)的服务，它是可以对 JavaScript 页面进行静态化。我们在 [prerender.io](http://prerender.io/) 网站上有专门的服务提供给大家，并且我们将源代码开放出来，因为我们认为 SEO 技术是属于大家的并不是属于个人的。

它可以结合一些中间件库来搭建一个预渲染 HTML 的服务给搜索引擎抓取。我们可以通过 Rails 或者 Node 开始学习它。

Prerender 支持 Google 的`_escaped_fragment_`，我们建议你这样使用。非常简单：

* 在你的页面的`<head>`中增加`<meta name="fragment" content="!">`标签；
* 如果你的站点采用锚点路有技术(#)，将它们改为(#!);
* OK！现在你的 JavaScript 页面可以完美支持 SEO 了；

Prerender 有很多插件，例如采用 Amazion S3 来缓存你的静态 HTML 页面。Prerender 还可以开启多个 phantomjs 进程来增加吞吐量。

## 中间件
Prerender 服务有很多中间件可以使用：

### 官方中间件
* Javascript
	* [prerender-node](https://github.com/prerender/prerender-node) (Express)
* Ruby
	* [prerender_rails](https://github.com/prerender/prerender_rails) (Rails)
* Apache
	* [.htaccess](https://gist.github.com/thoop/8072354)
* Nginx
	* [nginx.conf](https://gist.github.com/thoop/8165802)

### 社区中间件
* PHP
	* [zfr-prerender](https://github.com/zf-fr/zfr-prerender) (Zend Framework 2)
	* [YuccaPrerenderBundle](https://github.com/rjanot/YuccaPrerenderBundle) (Symfony 2)
	* [Laravel Prerender](https://github.com/JeroenNoten/Laravel-Prerender) (Laravel)
* Java
	* [prerender-java](https://github.com/greengerong/prerender-java)
* Go
	* [prerender-go](https://github.com/tampajohn/prerender)
* Grails
	* [grails-prerender](https://github.com/tuler/grails-prerender)
* Nginx
	* [Reverse Proxy Example](https://gist.github.com/Stanback/6998085)
* Apache
	* [.htaccess](https://gist.github.com/Stanback/7028309)

需要更多的不同框架的中间件请点击[issue](https://github.com/prerender/prerender/issues/12)。

## Prerender 是如何工作的
这是一个使用简单的服务，只需要输入一个 url 就可以得到被渲染好的 HTML（所有的`<script>`标签都去除了）。

注：这个请求需要通过你的服务器代理，这样那些引用的 CSS、图片等其它静态资源才能正常工作。

`GET http://service.prerender.io/https://www.google.com`

`GET http://service.prerender.io/https://www.google.com/search?q=angular`

## 在本地运行 Prerender
如果你想在本地测试 Prerender，那么你可以在本地运行 Prerender 服务，这样 Prerender 可以访问你本地开发的网站。

如果你在本地运行了 Prerender 服务，请确保你的中间件指向本地的 Prerender 服务：

```
export PRERENDER_SERVICE_URL=<your local url>
```

```
$ git clone https://github.com/prerender/prerender.git
$ cd prerender
$ npm install
$ node server.js
// also supports heroku style invocation using foreman
$ foreman start
```

Prerender 将运行在`http://localhost:3000`地址上。你可以访问`http://localhost:3000/http://localhost:8000`地址来查看你的网站最终的渲染的样子。

请记住你将会看到一些相对的 URL，因为访问的域名是你的 Prerender 服务器。这并不存在问题，因为一旦请求被代理到中间件，然后这个域名将会成为你的网站，并且那些请求不会发送到 Prerender 服务器。如果你想访问相对 URL 来访问网站，像这样访问：`http://localhost:8000?_escaped_fragment_=`

## 部署到 heroku

```
$ git clone https://github.com/prerender/prerender.git
$ cd prerender
$ heroku create
$ git push heroku master
```

如果将 Prerender 安装在 Windows 环境，你会遇到关于'node-gyp'的错误，你需要按照以下步骤解决：[https://github.com/nodejs/node-gyp#installation](https://github.com/nodejs/node-gyp#installation)。

## 定制化
你可以通过克隆这个项目来运行`server.js`，或者用`npm install prerender --save`来创建一个类似于 express 的服务。

## 插件
我们采用类似于 Connect 和 Express 的方式的一套插件系统，Prerender 的插件有一些不一样，并且我们不希望它与 prerender 中间件产生混淆，所以我们给他们命名为“插件”。

插件都存储在`lib/plugins`文件夹下面，它可以为 prerender 服务增加各种功能。

每个插件能够实现以下方法：

* init()
* beforePhantomRequest(req, res, next)
* onPhantomPageCreate(phantom, req, res, next)
* afterPhantomRequest(req, res, next)
* beforeSend(req, res, next)

## 如何使用插件
你可以通过解除在`server.js`中的注释来启用插件。

### prerender.basicAuth()
如果你知允许有权限的一方才能访问 Prerender 服务，启用 `basicAuth` 插件，并且设置用户名和密码即可：

```
xport BASIC_AUTH_USERNAME=prerender
export BASIC_AUTH_PASSWORD=test
```

然后确保能够通过身份验证（密码需要 base64 编码）。

```
curl -u prerender:wrong http://localhost:1337/http://example.com -> 401
curl -u prerender:test http://localhost:1337/http://example.com -> 200
```

### prerender.removeScriptTags()
去除 script 标签是因为我们并不想让已经渲染好的 HTML 再被 Javascript 框架渲染，爬虫或许不会执行脚本。

例如，如果一个基于 Angular 的页面已经被渲染成了 HTML，但是还保留 Angular 脚本在页面上，浏览器将在页面上有一次执行 Angular 进行渲染并且绑定数据。

这个插件实现了`beforeSend`方法，因此被缓存的 HTML 页面仍然包含 script 标签。

### prerender.httpHeaders()
如果脚本路由过程中发现了类似于404的错误，我们可以让 Prerender 服务给搜索引擎返回404，这样搜索引擎将不会收录这个页面。

如果想要返回 HTTP Headers，增加如下标签在页面的`<head>`中。注：Prerender 仍然发送页面的 HTML，它仅仅是改变了 HTTP Header 的状态码。

例如：让 Prerender 返回404页面：

```
<meta name="prerender-status-code" content="404">
```

例如：让 Prerender 返回302跳转：

```
<meta name="prerender-status-code" content="302">
<meta name="prerender-header" content="Location: http://www.google.com">
```

### prerender.whitelist()
如果只允许访问指定的域名，使用这个插件会让其它域名返回404。我们可以在这个插件中增加白名单域名，或者用`ALLOWED_DOMAINS`环境变量：

```
export ALLOWED_DOMAINS=www.prerender.io,prerender.io
```

### prerender.blacklist()
如果我们不允许指定的域名访问，使用这个插件会让这些域名返回404。我们可以在这个插件中增加黑名单域名，或者用`BLACKLISTED_DOMAINS`环境变量：

```
export BLACKLISTED_DOMAINS=yahoo.com,www.google.com
```

### prerender.s3HtmlCache()

### prerender.Region()
### prerender.inMemoryHtmlCache()
### prerender.logger()
### prerender.mongodbCache()
### prerender.memjsCache()
### prerender.levelCache()
### prerender.accessLog()

## 参考
https://github.com/prerender/prerender



