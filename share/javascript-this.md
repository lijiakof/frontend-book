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

## 总结
上面所说的可能很晕，记住这一点：用对象调用的方法中使用的`this`会指向该对象；直接调用的函数中使用的`this`会指向全局对象。

## 参考
* [Understanding JavaScript Function Invocation and "this"](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)
* [深入浅出 JavaScript 中的 this](http://www.ibm.com/developerworks/cn/web/1207_wangqf_jsthis/)

