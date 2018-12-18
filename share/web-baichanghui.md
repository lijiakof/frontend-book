# 百场汇 Web 前端

* 前端架构简介
    * 系统结构
    * 代码结构
    * 构建系统
    * 部署结构
    * 改进&问题
* lite 前端框架
    * lite.css
    * lite.js

## 前端架构简介
我们采用前后端分离的 SSR（Server Side Render） 架构来开发 Web&H5 应用，前后端只有接口的依赖，与传统的 Java、.NET、PHP 的网站开发有很大区别，我们更加接近 iOS 或者 Android 的应用程序开发。

与传统 Web 开发的不同：

* 纯正的前端开发：最终产物为 html+css+js；
* 独立运行：前端无需和 JSP 或者 ASPX 页面集成；
* 多环境运行：代码可以运行在浏览器、Node 甚至是 App 内部；
* 前端更加组件化、工程化：通过 Sass、Vue、Npm、Webpack 等框架和工具；
* 开发更加专注：一个问题或者功能不用从浏览器->应用服务器->缓存服务器->数据库；
* **前端工程师再也不是：只做将设计图转化成页面的工作了！**

### 前后端之间交互
采用前后端分离的开发模式后，更多的交互和逻辑会发生在浏览器端：

![前后端交互](https://webimg.baichanghui.com/reecho/articles/front-back-end.png)

* 浏览器从前端服务器获取 Html（模版） + Css + Javascript;
* 浏览器从后端服务器获取 Json（数据）；
* 浏览器通过 Html 和 Json 来渲染 Html；
* 用户和浏览器之间的交互；
* 浏览器提交数据到服务器。

### 系统结构
这一部分将会了解到整个网站系统的结构，我们横向分层，纵向分业务，纵观整个结构：从上往下越来越抽象，从左往右功能越来越丰富。引用关系由上至下，我们拒绝由下至上的引用：

* 语言&环境
* 框架层
* 业务公共层
* 应用层

![系统结构](https://webimg.baichanghui.com/reecho/articles/system-structure.png)

#### 语言&环境

* Browser & Node(PM2)
* Html
* Sass
* ECMAScript 6

环境：Web 前端的代码主要运行在浏览器端，但是也能在 Node 环境运行，通过 Vue-ssr Node 端插件，同样的前端代码也可以通过服务器端将 Html 渲染出来。正式的部署中，Node 的进程管理是通过 PM2（process manager 2），它可以帮你检查进程的健康情况，并提供强大的接口，让你很容易的了解 Node 在服务器中的运行情况，例如：

* pm2 start：启动服务
* pm2 stop：停止服务
* pm2 log：查看日志
* pm2 status：查看进程状态，可以看到进程使用的 cpu 、内存等
* pm2 m：实施监控进程状态
* 等...

语言：采用 Html 控制各个模块的结构；采用 Sass 做样式的预处理；采用 ECMAScript 6 来开发逻辑和交互，然后通过 Webpack 和 Babel 将高级版本的 JS 编译成当下流行浏览器能够解析的 ECMAScript 5。

#### 框架层
框架层主要解决：算法、存储、通讯和 UI 4 大问题。

* Vue
* lite.css
* lite.js
* lite-vue

核心框架采用 Vue 及其 Vue 系列插件： Vue-ssr 服务器端渲染模块，Vue-Router 路由模块，Vuex 数据流模块等；辅助工具库采用我们自己开发的 lite.js，内部封装了各种工具方法；样式框架也是采用自己开发的 lite.css。

基于 Vue 的插件机制，我们会将 lite 框架做成 Vue 的插件，这样就解决了框架中的 UI 问题。

##### Vue 2.0 和 Angular 2.0 之间的纠结
在框架选型的时候，我们考虑过 Angular 2.0，但是最终我们选择了 Vue 2.0。我们曾经用 Vue 和 Angular 作出了同样的一个页面进行对比：

* Vue 更加轻量级；
* Vue 入门成本更低；
* Vue 中文社区比较多，中文文档也翻译的很好；
* Vue 在 GitHub 中对问题的回复也很及时；
* Vue 语法更加忠于前端语言；
* Angular 功能更加全面；
* Angular 语法更加优美（TypeScript 是 Delphi 和 .NET 之父安德斯·海尔斯伯格设计）；

我们得出的结论是 Vue 更加适合互联网产品，Angular 2.0 更加适合大型企业应用（强烈推荐后端开发用它做 BOC）。

#### 业务公共层
主要解决每个站点的业务问题，例如：系统配置、获取用户信息、与后端接口的交互等。

* config
* services
* filters
* commons: user、channel、analytics...
* components: navbar、sigin、footer...

#### 应用层
以下是现在 Web 前端的各个业务站点：

* m.baichanghui.com
* v.baichanghui.com
* www.baichanghui.com
* passport.baichanghui.com
* pay.baichanghui.com
* my.baichanghui.com
* vender.baichanghui.com
* e.baichanghui.com
* d.baichanghui.com

### 代码结构

![代码结构](https://webimg.baichanghui.com/reecho/articles/code-structure.png)

* 整体结构&模块职责
    * assets
    * build
    * process
    * src
        * api
        * business
        * components
        * config
        * store
        * views
            * home
                * home.vue
                * home.banner.vue
    * package.json
* 模块命名规范：node_modules 不成文的规定
    * 全部小写
    * 单词之间用 `-` 隔开
    * 层次关系用 `.` 隔开
    * 使用有意义的名词
* 页面模块结构
    * `<template>`
    * `<script>`
    * `<style lang="sass">`

### 构建系统

![构建系统](https://webimg.baichanghui.com/reecho/articles/build-system.png)

* Git：源代码管理
* Npm：包管理
* Jenkins：持续集成工具，管理构建集成任务
* Webpack：前端构建工具
* Nexus：包管理

### 部署结构

![部署结构](https://webimg.baichanghui.com/reecho/articles/deployment-structure.png)

* Load balance
* Nginx
* Node + PM2
    
### 改进&问题

* Npm 私服
* Mobile 端调试工具
* 单元测试
* Api Mockup
* Store 模块重构
* 部署到 CDN：没有必要，因为版本变化
* 单页应用的横向扩展

### 总结一下前端技术要点

* Sass
* ECMAScript 6
* Node、Npm
* PM2
* Nginx
* Vue 及其插件
* Vue-ssr
* Webpack
* Git
* Jenkins
* lite
* **前端工程师再也不能：只做将设计图转化成页面的工作了！**

## lite 前端框架
提供一个轻量级的、易用的 js 库和 css 库。

特性：

* 轻便：min+gzip < 10k
* 容易使用
* 响应式
* 可定制
* 易扩展
* 兼容性：
    * iOS：Safari、Chrome
    * Android：Linux、Chrome
    * Windows：Chrome、Eadge、IE9+
    * macOS：Safari、Chrome
* 国际化

### litecss

* 基础样式
* 布局
* 基础组件
    * 容器组件
    * 导航组件
    * 状态组件
    * 表单组件

### litejs

* 基础库
* 设备&环境
* 计算
* 存储
* 通讯

### 欢迎一起开发
* https://www.litejs.org/
* https://www.litecss.org/
* https://github.com/lijiakof/litejs
* https://github.com/lijiakof/litecss

### 最后推销一下
去年在 GitHub 中写了一些关于 Web 前端的文章，帮忙 `star` 一下：https://github.com/lijiakof/frontend-book