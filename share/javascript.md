# 你所不知道的 JavaScript
## null 和 undefined 的区别
null和undefined的区别，两者值是相等的unll==undefined=>true；但是类型是不同的null!==undefined=>true：unll的类型是一个对象typeof(null) =>object，undefined的类型是一个"undefined"字符串typeof(undefined)=>"undefined"。
## NaN
NaN 在 JS 中是一个特殊的值，表示非数字（Not a Number），类型转换失败就会返回 NaN。NaN 的类型为数值类型 typeof(NaN) =>number。更奇怪的是NaN与所有值都不相等，包括自己 (NaN==NaN) => false。
## 全局变量和局部变量
通常人们认为在方法里面声明的变量都为局部变量，其实不然，如果在声明变量时不带有 `var` 其实无论它在方法内还是方法外都是全局变量；只有在方法里面声明变量时带上 `var` 才是真正的局部变量。例如：

```
<script>
	var a = 1;
	b = 2;
	function fn() {
		var c = 3;
		d = 4;
	}
</script>
```
其中 a、b、d 都为全局变量，只有 c 为局部变量
## Number 类型
在 JavaScript 中数字类型为浮点类型，所以在做小数的计算时候，经常会遇到不可思议的结果，这是由于精度的问题导致的：

```
var x = 0.3 - 0.2;
var y = 0.2 - 0.1;
console.log(x);
console.log(y);
```
最终会导致如下结果：

```
0.1
0.09999999999999998
```
## 1/0 = ? 0/0 = ?
或许很多 Web 前端程序员答不上这个问题，1/0 在 JS 的世界中会等于 Infinity，它是 Number 类型，表示无穷大；那么 0/0 呢？在 JS 的世界中它没有任何意义，运算的结果不是一个数字（Not a Number）也就是说结果会是 NaN，同样它也是一个 Number 类型。
## == 与 ===
`==`相等运算符号但是并不是严格意义的想等，如果两个不同类型的变量进行比较时，它会先尝试一些类型转换然后再进行比较；`===`严格意义的相等，比较过程中没有任何类型的转换。
## 真值与假值
JS 的世界中假值有 false、null、undefined、0、-0、NaN 和 ""，其它的都是真值；在做逻辑运算过程中不一定要使用或者返回布尔类型的值，也有可能使用或会返回一个“真值”或者“假值”。
## this