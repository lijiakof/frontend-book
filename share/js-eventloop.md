# Event Loop
JavaScript 代码的运行机制，主要依靠 Event Loop（事件循环）来实现的，在弄清楚整个机制之前，我们先要了解如下概念：

* single-thread（单线程）
* heap（堆内存）
* stack（栈内存）
* gc（垃圾回收）
* timers（定时器）
* macro-task or Task（宏任务）
* micro-task or Job（微任务）



* macro-task: script（整体代码）, setTimeout, setInterval, setImmediate, I/O, UI rendering
* micro-task: process.nextTick, Promises（这里指浏览器实现的原生 Promise）,Object.observe, MutationObserver


## 


* EventLoop setTimeout & Promise & Async
* https://juejin.im/post/5bfe581ee51d454af013baf3
* https://blog.csdn.net/lc237423551/article/details/79902106
* https://segmentfault.com/a/1190000011198232
* https://blog.csdn.net/eeewwwddd/article/details/80862682
* https://juejin.im/post/59e85eebf265da430d571f89
* https://vimeo.com/96425312
* http://www.ruanyifeng.com/blog/2014/10/event-loop.
* https://blog.csdn.net/weixin_42420703/article/details/82790942
* https://www.youtube.com/watch?v=cCOL7MC4Pl0
* https://www.cnblogs.com/yzg1/p/7514514.html
* https://mp.weixin.qq.com/s/qJSmotjzeu02EeK51NgFUQ?