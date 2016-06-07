# Promise 编程
Promise 是 CommonJS 中的规范，它能够帮助我们控制代码流程，避免函数的多层嵌套。现在 Web 前端异步编程越来越普遍，它的出现让异步编程变得更佳的容易理解。由于它越来越受到重视，ES6 已经开始支持它了。

## 什么是 Promise？
Promise 是一种异步编程的模型。它有三种状态：pending、resolved 和 rejected；一个 Promise 的状态只可能从 pending 转到 resolved 或者 rejected，不能逆向转换；同时 resolved 和 rejected 状态是不能相互转换的。

Promise 必须实现 then 方法，then 方法可以接受两个回调函数，第一个是操作成功时（状态变为 resolved）的回调，第二个是操作失败时（状态变为 rejected）的回调。

```
var promise = new Promise(function(resolve, reject) {
  // do a thing, possibly async, then…

	if (/* everything turned out fine */) {
		resolve("Stuff worked!");
	}
	else {
		reject(Error("It broke"));
	}
});

promise.then(function(result) {
	console.log(result); // "Stuff worked!"
}, function(err) {
	console.log(err); // Error: "It broke"
});
```

## Promise 的优缺点：

* [优势]在异步编程中即保证代码简洁（避免嵌套），又让代码有异步运行的能力；
* [缺点]无法取消 Promise，一旦新建它就会立即执行，无法中途取消；
* [缺点]如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部；
* [缺点]当处于 Pending 状态时，无法得知目前进展到哪一个阶段；

## ES6 中的 Promise
### Promise.prototype.then()
`then` 方法返回的是一个新的 Promise 实例，但是它并不是原来那个 Promise 实例。这样就可以采用链式的写法，即 `then` 方法后面在调用另外一个 `then`。

```
promise.then(function(){
	
}).then(function(){

});
```

### Promise.prototype.catch()
`catch` 方法是指发生错误时的回调函数：

```
promise.then(function(){
	
}).catch(function(){

});
```

### Promise.all()
`all` 方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

### Promise.race()
`race` 方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

### Promise.resolve()

### Promise.reject()

### done()

### finally()

## Promise 其它实现：

* [q](https://github.com/kriskowal/q)
* [bulebird](https://github.com/petkaantonov/bluebird)
* [co](https://github.com/tj/co)
* [then](https://github.com/cujojs/when)

## 参考：

* https://promisesaplus.com/
* http://www.jianshu.com/p/063f7e490e9a
* https://www.promisejs.org/
* https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise
* http://es6.ruanyifeng.com/#docs/promise
* http://www.cnblogs.com/dojo-lzz/p/4340897.html




