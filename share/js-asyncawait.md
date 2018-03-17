# Async/Await
异步编程是 Web 前端工程师最难处理的问题之一，自从 Ajax 的出现后，异步编程越来越普遍，现在前端 MV* 框架的兴起让前端完全和后端分离开来，使得异步编程已经司空见惯，异步编程也是从回调函数、到事件的监听、到 Promise、到 Generator 函数，它们都在解决同样一个问题，让异步编程的调用更佳的“扁平”化，让前端工程师在异步编程中更佳容易和优美的解决问题，终于 Async/Await 出现了。

## Async/Await 是什么
Async/Await 是编写异步程序的新方式，它实际上是基于 Promise 来实现的，它会让异步的代码写起来和同步代码一样。Node 已经在 7.6 版本开始支持 Async/Await，并且它已经在 ES7 的新特性中出现。

## 如何使用
让我们看看它到底是如何使用的，它采用了什么样神奇的语法糖。在揭开它的面纱前，我们先看看一个使用 Promise 的异步编程：我们用 `axios` 模块异步获取一条数据：

```
function getData() {
    return axios.get('http://domain/path')
        .then(resp => {
            return resp;
        });
}

getData();
```

如果使用 Async/Await：

```
async function getData() {
    resp = await axios.get('http://domain/path');

    return resp;
}

getData();
```

第一种写法我们已经很熟悉，采用 Promise 的写法，将移步的调用编程了链式的写法，让复杂的回调更佳的清晰。让我们再看看第二种 Async/Await 写法：

* `函数`前增加关键字 `async`，在`函数`体内使用了 `await` 关键字，`await` 当然它也只能声明在`函数`体内。该函数会隐式地返回一个 `Promise` 对象，`函数`体内的 `return` 值，将会作为这个 `Promise` 对象 `resolve` 时的参数。
* `await axios.get()` 会让`函数`等到 `axios.get()` 返回的 `Promise` 对象 `resolve` 之后触发。

## 使用 Async/Await 的 6 大优势
我们参考了 [Mostafa Gaafar](https://hackernoon.com/@mgaafar) 高级全栈工程师的文章，来解释一下 Async/Await 如下 6 大优势：

### 让代码更佳的简约
回顾一下我们上面的代码实例中，很明显我们的代码简洁清晰了很多。我们不用写 `then` ，不需要创建一个匿名函数来处理返回值，或者也不需要把一个数据赋给一个我们不需要的值。我们甚至是避免了代码的嵌套。这些小的优势将会在下面的代码中更加明显。

### 错误处理


## 参考
* https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9?gi=67cc4f92c3ef