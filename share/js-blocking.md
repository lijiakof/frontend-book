# 如何阻断运行中的 JavaScript
通常情况下 JavaScript 是单线程的，一段 Js 程序的允许会占满整个程序进程，我们通常会想方设法的通过异步来减少阻塞，那么我们今天反其道而行之，看看通过怎么样的**正常的**方式可以阻塞 Js 允许。

## 无限循环
单线程的 JavaScript 可以给我们灵感，只要程序不断的计算就可以阻塞程序的进程：

```
function sleep(d){  
    var t = Date.now();
    while(Date.now() - t <= d);  
}

function test() {
    console.log('sleep');
    sleep(10000);
    console.log('run');
}

test();

```

但是这种方式其实是通过无限占用计算机的资源来造成假死状态，它会消耗大量的 CPU，并没有真正的让程序进程停止，这中方式不可取。

## setTimeout
我们直接用 setTimeout 回调的方式来阻断程序的进程，当然它的确是没有让程序继续进行并且让 CPU 空闲下来，但是这种写法并不是一种同步编程的方式：

```
function test() {
    console.log('sleep');
    setTimeout(function() {
        console.log('run');
    }, 10000)
}

```


## await
ES 的高级版本出现了 Promise、await 等异步编程，它们让程序的写法更佳的优雅简介，同样也要借助于 setTimeout 来解决，建议采用此方式：

```   
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function test() {
    console.log('sleep');
    await sleep(10000);
    console.log('run');
}

test();

```

## generator & yield
ES6 的迭代器同样也具备异步编程能力，但是这种写法相当晦涩，建议少用：

```
function sleep(time) {
    setTimeout(function () {
        test.next();
    }, time);
}

function* gen() {
    console.log('sleep');
    yield sleep(10000);
    console.log('10 second later');
}

var test = gen();
test.next();
```

## 总结
上述方法总结下来就是两种，一种是强行阻断式利用 Js 的单线程机制；另一种就是借助 Js 的异步事件机制+高级异步编程语法。当然我们在实际业务情况中使用阻断 JS 进程的地方非常少，经常会通过 UI 来禁止用户继续操作，这样的探索仅仅是搞清楚一些 Js 的基本原理，有助于我们很好的了解它。
