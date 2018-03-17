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
    let resp = await axios.get('http://domain/path');

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
回顾我们上面的代码实例中，很明显我们的代码简洁清晰了很多。我们不用写 `then` ，不需要创建一个匿名函数来处理返回值，或者也不需要把一个数据赋给一个我们不需要的值。我们甚至是避免了代码的嵌套。这些小的优势将会在下面的代码中更加明显。

### 更容易处理错误
Async/Await 最终让以同样的方式来处理同步和异步的错误成为了可能，让 `try/catch` 更好的使用。下面的实例中，使用 Promise，来处理异步编程的错误捕获，由于程序是异步的最外面一层 `try/catch`，它无法捕获到 Promise 内部发生的错误，我们只能再嵌套一层 `try/catch` 来解决：

```
function getData() {
    
    try {
        axios.get('http://domain/path')
            .then(resp => {
                try {
                    let json = JSON.parse(resp);
                }
                catch (err) {
                    console.log(err);
                }

                return json;
            });
    }
    catch (err) {
        console.log(err);
    }
}
```

我们看看使用了 Async/Await 的异步编程是如何处理错误的：

```
async function getData() {
    let data;

    try {
        data = JSON.parse(await axios.get('http://domain/path'));
    }
    catch (err) {
        console.log(err);
    }

    return data;
}

```

## 更容易处理条件判别
想象一下这样的业务逻辑：我们先要获取一些数据，然后通过获取到的数据来判断是否继续获取更多的详细数据：

```
function getData() {
    return axios.get('http://domain/path').then(resp => {
        if(resp && resp.data && resp.data.yes) {
            return axios.get('http://domain/path1').then(resp1 => {
                return resp1;
            })
        }
        else {
            return resp
        }
    });
}
```

这样的写法会让你很头疼，嵌套太多会导致很多变量的遗失，让逻辑难以处理。接下来让我们看看 Async/Await 编程如何让代码更佳容易阅读：

```
async function getData() {
    let data = await axios.get('http://domain/path');

    if(data.yes) {
        let data1 = await axios.get('http://domain/path1');
        return data1;
    }
    else {
        return data;
    }
}
```

## 更容易处中间值
你可能会发现自己会有这样的情况：你调用了 `promise1` 然后将它返回给 `promise2`，最后又奖两个 promise 的结果都返回给 `promise3`。你的代码很可能会写成这样：

```
function getData() {
    return promise1()
        .then(value1 => {
            return promise2(value1)
            .then(value2 => {        
                return promise3(value1, value2)
        })
    })
}
```

同样的逻辑我们用 Async/Await 编写：

```
async function getData() {
    const value1 = await promise1()
    const value2 = await promise2(value1)
    return promise3(value1, value2)
}
```

## 错误堆栈信息
想象一下通过多个链式调用 Promise ：

```
function getData() {
    return callAPromise()
        .then(() => callAPromise())
        .then(() => callAPromise())
        .then(() => callAPromise())
        .then(() => callAPromise())
        .then(() => {
        throw new Error("oops");
        });
}

getData().catch(err => {
    console.log(err);
    // output
    // Error: oops at callAPromise.then.then.then.then.then (index.js:8:13)
})
```

此链条的错误堆栈信息并没用线索指示错误到底出现在哪里。更糟糕的是，它会误导我们：

## Debugging
最后，也是最重要的优势是，使用 Async/Await 非常容易调试，调试 Promise 有如下两个不好的地方：

* 我们不能在返回表达式的箭头函数中设置断点；
* 如果你在 `.then` 中设置断点，然后通过 `step-over` 的方式进行调试，调试器不会移动到下一步，因为它只能通过同步的方式移动到下一步。

但是如果我们使用 Async/Await 你可以一步一步的正常的使用调试。

## 最后
Async/Await 是 JavaScript 中添加的具有革命性特征之一，它让你更容易的进行异步的编程。

## 参考
* https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9?gi=67cc4f92c3ef