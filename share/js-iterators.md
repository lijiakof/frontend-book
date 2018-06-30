# ES6 迭代器
ES6 引入了一个全新的迭代器的概念，它提供一种方法能够顺序访问一个聚合对象中的各个元素，而不需要暴露该对象的内部表现。

首先我们回顾一下前一代 JS 提供的类似于迭代器功能的 `for-in` 语法，以便于我们很好的理解迭代器的基本原理：

```
for (var key in list) {
    console.log(key + ' = ' + list[key]);
}
```

`for-in` 最大的问题是，它不保证迭代的顺序，但 ES6 提供的迭代器解决了这个问题。

## `for-of`
`for-of` 是 ES6 提供的行语法，它是和迭代器配合使用的：

```
for (let value of list) {
    console.log(value);
}
```

我们希望得到一个确保顺序的迭代，为了确保对象可以迭代，它需要实现一个可迭代的协议，即拥有 `Symbol.iterator` 属性，这个属性将会被 `for-of` 所使用。list[Symbol.iterator] 的值必须是符合迭代协议的函数，它需要返回一个类似于：` { next: function() {} } ` 的值：

```
list[Symbol.iterator] = function () {
    return {
        next: function() {

        }
    }
}
```

当 `next()` 函数被 `for-of` 循环调用时，它需要返回一个类似于 `{value: ..., done: [true/false]}` 的对象。这样一来我们实现的有序的迭代器如下：

```
list[Symbol.iterator] = function () {
    let keys = Object.keys(this).sort();
    let index = 0;

    return {
        next: function() {

            return {
                value: keys[index], 
                done: index++ >= keys.length
            };
        }
    }
}
```

## 惰性执行（Laziness）
回顾上面的例子，当我们调用迭代器的瞬间，就立即执行了排序等逻辑工作，但是如果 `next()` 函数永远不用调用的话，这部分排序等逻辑会造成不必要的计算机资源浪费，我们如下优化：

```
list[Symbol.iterator] = () => {
    let keys = null;
    let index = 0;

    return {
        next: () => {
            if(keys == null) {
                keys = Object.keys(this).sort();
            }

            return {
                value: keys[index], 
                done: index++ >= keys.length
            };
        }
    }
}
```

这样一来，只有在调用 `next()` 函数时才会进行排序等逻辑的操作，并且它会被缓存到 `keys` 变量中，这样一来排序是被动触发的，只有在需要的时候才会使用，这就是所谓的`惰性执行（Laziness）`，类似于我们在 WEB 前端的图片`懒加载（Lazy load）`一样。

## `for-in` vs `for-of`
直接上例子：

```
var list = [3, 5, 7];
list.foo = 'bar';

for (var key in list) {
  console.log(key); // 0, 1, 2, foo
}

for (var value of list) {
  console.log(value); // 3, 5, 7
}
```

其实，`for-in` 并不是我们想要的结果。

## ES6 内置迭代器
在 ES6 中 `String`、`Array`、`TypedArray`、`Map` 和 `Set` 都是内置的迭代器，因为它们都被实现了 [Symbol.iterator]：

```
Array.prototype[Symbol.iterator]; // ƒ values() { [native code] }
```

## 无限迭代器
只要 `next()` 函数永远不返回 `{ done: true }`， 就可以实现无限迭代器了，但是这是需要避免的情况。

## 最后
迭代器给 `循环`、`generator`、`值序列`带来了全新的纬度。你可以通过它来定义一个类，来确定值的排序方式，也可以通过它来创建一个惰性或者无限循环的序列。

## 参考
* https://developer.ibm.com/node/2015/07/16/introduction-to-es6-iterators/
