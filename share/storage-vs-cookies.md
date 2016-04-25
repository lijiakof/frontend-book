# HTML5 的本地存储
HTML5 新增本地存储 LocalStorage 和 SessionStorage，利用本地存储会在数据层面上给 Web 前端带来性能上的提高，但是它的容量相对来说是有限的，在选择存储哪些数据和定期的清理数据需要一些考究。

# LocalStorage vs SessionStorage
两者的区别在于存储的有效期限，LoaclStorage 存储是永久的，除非手动清除；然而 SessionStorage 的存储是浏览器会话级别，如果浏览器关闭了就消失了。

## 用法
* localStorage.getItem(key)
* localStorage.setItem(key, value)
* localStorage.removeItem(key)
* localStorage.clear()
* sessionStorage.getItem(key)
* sessionStorage.setItem(key, value)
* sessionStorage.removeItem(key)
* sessionStorage.clear()

# 本地存储 vs Cookies
本地存储和 Cookies 区别在于

* 存储大小，本地存储官方说法是 5M 的大小，然而 Cookies 的存储大小更小并且和浏览器本身有关（大概在 4000个字节左右）
* Cookie 存储的内容会保留在 HTTP 请求的 Header 中，并且会随每次请求发送到浏览器；然而本地存储只会保留在客户端，浏览器请求过程不会发送到服务端





