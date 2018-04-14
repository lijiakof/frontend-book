# 如何优雅的处理 `async/await` 错误
`Async/await` 是 ES7 中的新特性，它可以让开发者编写异步代码像同步代码一样，它的优势我们通过 [Async/Await](js-asyncawait.md) 这篇文章来了解。

的确它给我们带来了很多方便的地方，但是在 async/await 中如何来处理错误呢？在异步的调用中，会产生各种不同的错误，例如：HTTP 请求产生了错误、访问 BD 产生的异常、操作文件产生异常。在 `Promise` 的使用中，当承诺遇到了错误，它会抛出一个异常，该异常将被捕获到一个方法回调中。在 `async/await` 中，我们又如何处理呢，当然很多人会回答：使用 `try/catch` 来捕获这些错误，这样一来代码会看起来像这样：

```
async function asyncFunc() {
    try {
        const product = await Api.product({ id : 10 });
        if(!product) {
            console.log('No product found');
        }
    }
    catch(err) {
        console.log(err);
    }

    try {
        const saveProduct = await Api.save({
            id: product.id,
            name: product.name
        });
    }
    catch(err) {
        console.log(err);
    }

}
```

回顾上面的代码，`try/catch` 的确可以来解决错误异常的处理，但是让代码非常的不干净，原本 `async/await` 的优势就是让代码更佳的简约，这样一来又违背了它的初中，这让我们进入了新的思考。

## Go-lang 的灵感
在 Go 语言中处理异常的方式是这样的：

```
f, err := os.Open("filename.txt")

if err != nil { return err }
```

它看起来要比繁多的 `try/catch` 更佳的干净，并且让代码更佳容易阅读。我们是不是可以把这种语法运用到 `async/await` 中去呢，但是让人失望的是 async/await 如果产生了错误会立即退出你的函数，除非用 `try/catch`，否则你无法控制它。

但是没有我们聪明的工程师无法办到的事情，Dima 和他的小伙伴利用 Promise 来解决了这个问题：

```
// to.js
export default function to(promise) {
   return promise.then(data => {
      return [null, data];
   })
   .catch(err => [err]);
}
```

这一个工具方法接收一个承诺，让后将异步获取到的数据作为`返回数组`的第二个值，捕捉到的错误作为`返回数组`第一个值。然后我们的异步代码会变成这样：

```
import to from './to.js';

async function asyncFunc() {
    let err, product, saveProduct;

    [err, product] = await Api.product({ id : 10 });
    if(!product) {
        console.log('No product found');
    }

    [err, saveProduct] = await Api.save({
        id: product.id,
        name: product.name
    });

    if(err) {
        console.log(err);
    }
}

```

由此一来，`async/await` 又回到了最初的简洁。

## await-to-js 
最后给 `await-to-js` 点一个赞，它很好的将这一工具封装成了模块，你可以通过 npm 来安装它，源码地址：https://github.com/scopsy/await-to-js ，代码其实很少，用 Typescript 写的，小功能大作用。

```
/**
 * @param { Promise } promise
 * @param { Object= } errorExt - Additional Information you can pass to the err object
 * @return { Promise }
 */
export function to<T, U = any>(
  promise: Promise<T>,
  errorExt?: object
): Promise<[U | null, T | undefined]> {
  return promise
    .then<[null, T]>((data: T) => [null, data])
    .catch<[U, undefined]>(err => {
      if (errorExt) {
        Object.assign(err, errorExt)
      }

      return [err, undefined]
    })
}

export default to
```

## 参考
* https://blog.grossman.io/how-to-write-async-await-without-try-catch-blocks-in-javascript/