# HTML meta
很多前端开发对 HTML 的`<meta>`标签视而不见，经常会忽略它的存在，认为它没有太大的作用，现在我们就谈谈`<meta>`这个标签的使用。

## meta 标签的作用

* 搜索引擎（SEO）优化；
* 定义页面使用语言；
* 自动刷新页面；
* 控制页面缓存；
* 网页定级评价；
* 控制页面显示的窗口；
* 等等...

## meta 使用
`<meta>`标签总共有 3 个属性，不同的属性和值组成了网页不同的功能：

* name
* http-equiv
* content

### name 属性
`name`属性主要是用于描述网页的，对应`content`属性中的内容是便于搜索引擎查找和分类信息。语法：

```
<meta name="" content="" />
```

#### name="keywords"
它是用来设置，让搜索引擎获取网页的关键字：

```
<meta name="keywords" content="活动,聚会，拓展，团建，培训，讲座" />
```

#### name="description"
它是用来设置，让搜索引擎获取网页的内容描述：

```
<meta name="description" content="百场汇是中国最大的会议活动和工作场地短租平台，提供场地直销服务，价格超低，无任何附加费用，帮助用户寻找各种各样的特色场地。" />
```

#### name="robots"
它是用来设置，让搜索引擎哪些页面需要索引，哪些页面不需要索引。`content` 有如下参数：

* all：文件将被检索，且页面上的链接可以被查询；
* none：文件将不被检索，且页面上的链接不可以被查询；
* index：文件将被检索；
* noindex：文件将不被检索，但页面上的链接可以被查询；
* follow：页面上的链接可以被查询；
* nofollow：文件将被检索，但页面上的链接不可以被查询。

```
<meta name="robots" content="all" />
```

#### name="author"
它是来设置页面的作者：

```
<meta name="author" content="jay" />
```

#### name="generator"
它是来设置网站采用什么软件制作的：

```
<meta name="generator" content="hobbit" />
```

#### name="COPYRIGHT"
它是来设置网站的版权信息的：

```
<meta name="COPYRIGHT" content="百场汇" />
```

#### name="revisit-after"
它是来设置网站的重访，`30day`代表30天：

```
<meta name="revisit-after" content="30day" />
```

### http-equiv 属性
`http-equiv`相当于 HTTP 的文件头的设置。语法：

```
<meta http-equiv="" content="" />
```

#### http-equiv="expires"
它是来设置网页的过期时间的：

```
<meta http-equiv="expires" content="Fri May 13 2016 22:49:44 GMT+0800 (CST)" />
```

#### http-equiv="Pragma"
它是来设置禁止浏览器从本地缓存中访问页面：

```
<meta http-equiv="Pragma" content="no-cache" />
```

#### http-equiv="Refresh"
它是来设置自动刷新并跳转新页面，其中`content`第一个数字代表 5 秒后自动刷新：

```
<meta http-equiv="Refresh" content="5;URL=http://m.baichanghui.com" />
```

#### http-equiv="Set-Cookie"
它是来设置 Cookie 的：

```
<meta http-equiv="Set-Cookie" content="cookie value=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/" />
```

#### http-equiv="Window-target"
强制页面在当前窗口以独立页面显示：

```
<meta http-equiv="Window-target" content="top" />
```

#### http-equiv="content-Type"
它是来设置页面使用的字符集：

```
<meta http-equiv="content-Type" content="text/html;charset=gb2312" />
```

#### http-equiv="Content-Language"
它是来设置页面的语言的：

```
<meta http-equiv="Content-Language" content="zh-cn" />
```

#### http-equiv="Cache-Control"
它是设置页面缓存：

```
<meta http-equiv="Cache-Control" content="no-cache" />
```

**这里可以设置很多不同的缓存机制**

#### http-equiv="Content-Script-Type"
它是设置页面中脚本的类型：

```
<meta http-equiv="Content-Script-Type" content="text/javascript" />
```






