# ECMAScript 6
ECMAScript 6 是 JavaScript 的第 6 个版本，已经在 2015年6月正式发布，它的目标是让 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。
## Babel 转码器
Babel 是一个广泛使用的代码转换器，可以讲 ES6 转换成 ES5，这意味着里可以使用 ES6 的方式编写代码，不用担心环境是否支持，通过它可以将 ES6 语法转换成 ES5 的语法。例如：

```

``` 
## ES6 vs ES5
### let vs var
ES6 新增了`let`命令，用来声明变量，与`var`不同的是，用它声明的变量只有在`let`所在的代码块内有效。例如：

```
{
	let a = 1;
	var b = 2;
}
console.log(a);//Uncaught ReferenceError: a is not defined
console.log(b);//2
``` 
这意味着通过`let`声明的变量就像在闭包中的变量一样，外部无法访问，并且`let`声明的变量不像`var`声明的变量会发生**变量提升**现象，所以用`let`声明的变量一定要在声明后才能使用，否则会报错。

```
console.log(a);//Uncaught ReferenceError: a is not defined
console.log(b);//undefined
let a = 1;
var b = 2;
``` 
在代码内使用`let`声明的变量，该变量不能不能在声明之前使用，在语法上称之为：Temporal Dead Zone（TDZ，暂时性死区）。ES6之所以规定TDZ，主要是为了减少运行时错误，防止在变量声明前使用这个变量，从而导致意外的行为。

另外`let`事实上为 JavaScript 新增了**跨级作用域**，这样只有子作用域能够访问到父级作用域的变量，父级是不能访问子级的，这样我们可以采用这种方式来替换 ES5 时代的闭包。

### 新增 const
`const`也是用来声明变量，但是声明的变量为常量，常量的值是不能改变的。并且一旦声明就需要赋值，否则会报错；

```
const PI = 3.1415926;
console.log(PI);//3.1415926

PI = 3.14;//Uncaught TypeError: Assignment to constant variable.

const G;//Uncaught SyntaxError: Missing initializer in const declaration
```

### 全局对象的属性
全局对象是最顶层的对象，在浏览器环境是 window 对象，在 Node.js 中是 global 对象，ES5 中全局对象的属性与全局变量是等价的，这被视为 JavaScript 语言的一大问题，因为很容易就不知不觉创建了一个全局变量。ES6 改变了这一点，通过`var`和`function`声明的全局变量依旧是全局对象的属性，而`let`、`const`、`class`声明的变量不属于全局对象的属性。

```
var a = 1;
window.a;//1

let b = 2;
window.b;//undefined
```

### 数组的解构赋值
### String 的扩展
### RegExp 的扩展
### Number 的扩展
### Array 的扩展
### Function 的扩展
### Object 的扩展
### Symbol
### Proxy 和 Reflect
### 二进制数组
### Set 和 Map
### Iterator 和 for...of





