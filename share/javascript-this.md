# JavaScript 中的 this
在很多面向对象的语言中（Java、C#），this 关键字的含义非常明确，就是代表当前对象，通常在编译期间就确定下来（编译期绑定）；而在 JavaScript 中，this 是动态绑定的，这样导致 this 非常灵活多变，给我们带来非常多的不便。

## 外部函数调用
外部函数中调用`this`时，它将指向全局对象，浏览器中即为 window：

```
function whatIsThis() {
	console.log(this);
}
whatIsThis();
```
会打印出`Window`对象

## 内部函数调用
内部函数中调用`this`时，它和外部函数调用一样，指向了全局对象：

```
var obj = {
	a: 0,
	fn: function(){
		var whatIsThis = function(){
			console.log(this);
		}
		
		whatIsThis();
	}
};
obj.fn();
```
会打印出`Window`对象

## 对象的方法调用
对象的方法中调用`this`时，它将指向这个对象：

```
var obj = {
	a: 0,
	whatIsThis: function(){
		console.log(this);
	}
};
obj.whatIsThis();
```
会打印出当前`Object`对象

## 构造函数内部调用
构造函数的对象调用`this`时，它将指向这个对象：

```
function Class() {
	console.log(this);
}
var obj = new Class();
```

## 使用 call 和 apply 调用

## 箭头函数中的 `this`
在 ES6 语法中 `()=>this` 箭头函数中的 `this` 就很特殊了，箭头函数没有一个自己的 `this`，但当你在内部使用了 `this`，常规的局部作用域准则就起作用了，它会指向最近一层作用域内的 `this`。

```
var obj = {
	a: 0,
	whatIsThis: function() {
		console.log(this);
		
		var fn = () => console.log(this);
		
	}
};
obj.whatIsThis();
```

两个都会打印出当前`Object`对象

## 总结
上面所说的可能很晕，记住这一点：用对象调用的方法中使用的`this`会指向该对象；直接调用的函数中使用的`this`会指向全局对象。

## `this` 是否可以被完全修改（或者把一个对象全部赋值给它）？
昨天面试的时候，突然有一个自称初学者的应聘同学问了一个问题，`this`是否能给它赋上另外一个对象呢？我当时就傻了，在实际操作中很少去做这件事情，将方法内部的`this`增加一个属性倒是没有太大问题，如果试图全部修改它，这种事情恐怕很少做这种操作。当然我当时的回答是：“好像有点矛盾，很可能 this 本身是有一些属性是只读的”。

于是我回来做了一个实验，将各种情况的 `this` 都赋上另外一个对象，结果是：*Uncaught ReferenceError: Invalid left-hand side in assignment* 。看起来的确是不行的，细想一下也是能够理解的：

* 如果自己能够将自己变成一个其它的对象，但是另外那个对象又没有修改自己的能力，这岂不是自相矛盾；
* 如果另外一个对象的确能修改自己，这样一来岂不是进入了无限被修改的循环中。

## 参考
* [Understanding JavaScript Function Invocation and "this"](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)
* [深入浅出 JavaScript 中的 this](http://www.ibm.com/developerworks/cn/web/1207_wangqf_jsthis/)

