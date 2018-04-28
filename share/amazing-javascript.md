# 你所不知道的 JavaScript II *（手把腿的教你 JS）*
### 初识 JavaScript
JavaScript 出生于 1995 年，目前它服务在 92% 左右的网站上面，GitHub 开源社区中 20% 左右的项目采用了 JavaScript。在互联网兴起的年代 JavaScript 发展迅猛，以 IE 为首的微软公司开始制定起自己的 JavaScript 标准，包括火狐、Opera 等，这时候标准泛滥，开发人员为了兼容各个版本浏览器每天加班加点，代码也越来越难以维护。后来 ECMA（欧洲计算机制造联合会，一个统一电脑操作格式标准、程序语言和输入输出的组织）创建了 ECMAScript（ES）标准，现在越来越多的浏览器开始遵从这一标准，此时的 WEB 前端语言才开始真正的统一起来，现在浏览器大部分都兼容 ES5（第五个版本）标准，ES6 在 2015 年已经发布，现在很多工程师已经开始使用新的标准开发，相信 JavaScript 未来会越来越完善越来越流行。

JavaScript 这种弱类型的语言，入门很简单，但是想玩转它需要下一些功夫，因为它和常规语言的语法有很多不同之处，通常不安常理出牌，如果不摸清楚它的性格，可能在出现 bug 的时候会困扰你半天。我从众多 JavaScript 反常规思维的语法中挑选了一些例子，知道了这些后，会帮助你更好的解决实践过程中的问题。

### JavaScript 和 Java 的区别
很多人会将两者混为一谈，其实它们没有血缘关系！

* 出生不同：JavaScript 是 Netscape 公司所生；Java 是 Sun 公司所生；
* 编译不同：JavaScript 是“动态”或“解释执行”语言；Java 是编译型语言；
* 类型不同：JavaScript 是弱类型的；Java 是强类型的；
* 对象不同：JavaScript 的面相对象是基于原型的；Java 是基于类的；

![JavaScript vs Java](resources/javascript-vs-java.png)

*JavaScript 和 Java 的关系好比仓鼠和火腿的关系*

### 混沌中的全局变量和局部变量
*写给非工程师的备注：变量的声明好比在医院挂号，声明不同类型的变量，就好比你挂不同类型的号。那么作用域就好比医院，不可能你在民航医院挂的号能在朝阳医院也可以用*

```
var a = 1;
b = 2;

function fn1() {
    var c = a;
    d = 4;
}

function fn2() {
    console.log(d);
}

fn1();
fn2();

```

运行这段代码，居然会打印出 `4`，后端 Java 工程师志兵吓一大跳，不可理喻，并且会对 JavaScript 语言的设计者破口大骂：“啥xx语言！”。常见的计算机语言中，变量是有一定的作用域的，在作用域内部的变量只会在这个作用域内部有效果，超出它的作用域后就无法使用了。

但是奇葩的 JavaScript 就是和别人不一样，`d` 这个变量仿佛穿越了时空，原来在 JavaScript 中，变量的声明有如下特征：

* 不用 `var` 声明的变量（隐式声明）都会附属在全局变量 `window` 下面；
* 所有变量的声明都会被提升到该变量作用域的最前面；

```
// begin: 变量的声明提升
var a;
window.b;
window.d;
// end: 变量的声明提升

a = 1;
window.b = 2;

function fn1() {
    // begin: 变量的声明提升
    var c;
    // end: 变量的声明提升

    c = 3;
    window.d = 4;
}

function fn2() {
    console.log(window.d);
}

fn1();
fn2();
```

#### 课后作业：先有鸡还是先有蛋？
有了变量声明会被提升的知识后，看看下面两段代码，它们运行的结果如何？

```
a = 2;
var a;
console.log(a);
```

```
console.log(a);
var a = 2;
```

### 生死冤家 `null` 和 `undefined`
`null` 和 `undefined` 到底什么区别，经常搞得我们非常苦恼，判断一个变量存不存在我们该选谁？

* `null` 已经声明，已经定义，只是没有内容：病了，已经到医院挂号了，只是还在等候医生就诊。
* `undefined` 已经声明，但是未定义：病了，还没挂号，还在医院排队挂号。
* `null` 是 `object` 类型；
* `undefined` 是 JavaScript 中的特殊类型 `undefined`；
* `(null == undefined) => true` 值相等；
* `(null === undefined) => false`；

```
var a = null;
var b;

console.log(a);
console.log(b);

console.log(typeof a);
console.log(typeof b);

console.log(a == b);
console.log(a === b);
```

原来，这与 JavaScript 的历史有关。1995 年 JavaScript 诞生时，最初像Java一样，只设置了 `null` 作为表示"无"的值。根据 C 语言的传统，null 被设计成可以自动转为 0。但是 JavaScript 的设计者 Brendan Eich，觉得这样做还不够，于是又设计了一个 `undefined`。

#### 课后作业：`null` 、 `undefined` 和 `0` 的比较?
我们知道 `(null == undefined) => true` 它们两者值是相等的，那么：

```
console.log(null == 0);
console.log(undefined == 0);
```

### `1 + - + - - + + + - + 1` 什么鬼？
看到如此神奇的运算，想必小伙伴们都惊呆了，是的！你看到的这一串在 JavaScrpit 中是可以运行起来的。运算还可以如此玩？

如何计算：

* 一元运算的优先级要高于二元运算；
* 一元加和一元减运算是从右向左的顺序来计算的：

```
1 + - + - - + + + - + 1
1 +(-(+(-(-(+(+(+(-(+(1))))))))))
```

总结规律：

* 所有的 `+` 都忽略；
* 看 `-` 的奇偶：
    * 如果是奇数，那么等效于 `1 - 1`；
    * 如果是偶数，那么等效于 `1 + 1`；

### 琢磨不透的数字之间的计算

```
var a = 0.2 - 0.1; // => 0.1
var b = 0.3 - 0.2; // => 0.09999999999999998
```

怎么会这样！我喝醉了吗？刚刚上线完的长顺不敢相信自己的眼睛，抓耳挠腮，这可咋办？又要上一次线了！

这是由于精度造成的，JavaScript 的数字（无论是整数还是小数）采用的是64位双精度浮点类型，存储方式如下：

1位符号位+11位指数位+52位小数位

当在运算过程中超过了存储的位数，将会丢失因为丢失数据产生精度问题。

* 先转换成二进制
* 二进制计算
* 转换成十进制

```
0.1 => 0.0001 1001 1001 1001…（无限循环）
0.2 => 0.0011 0011 0011 0011…（无限循环）
```


### `NaN` What the xxxx is this?
`NaN` 性格非常纠结，它是数字类型，但是又代表不是一个数字(`Not a Number`)，当在做计算时产生错误通常会得到 `NaN` 这个结果；这位哥们性格也非常孤僻，孤僻到和自己都合不来。

* 它是一个数字类型；
* Not a Number；
* 它和任何数字进行比较运算的结果都是 `false`，甚至是自己；

```
console.log(typeof NaN); // => number
console.log(NaN == 1);   // => false
console.log(NaN > 1);    // => false
console.log(NaN < 1);    // => false
console.log(NaN == NaN); // => false
console.log(NaN > NaN);  // => false
console.log(NaN < NaN);  // => false
```

#### 课后作业：`NaN != NaN` ?

### `1/0` 和 `0/0`这也行？
在很多语言中 `0` 是不能被除的，甚至都无法通过编译，但是神奇的 JavaScript 不会报错，并且会给你一个满意的结果：

* `1/0 => Infinity`：
    * `Infinity` 是 JavaScript 中的一个特殊的值，表示无穷大；
    * `Infinity` 是一个数字类型；
    * `Infinity > 100000000 => true` 但是 `Infinity > NaN => flase`（PS：`NaN` 就是屌！）；
* `0/0 => NaN`；

### 变幻莫测的 `this`
在很多语言中都有 `this` 这个关键词，它提供了一种优雅的方式来隐式传递一个对象引用，在很多面向对象的语言中（Java、C#），`this` 关键字的含义非常明确，就是代表当前对象，通常在编译期间就确定下来（编译期绑定）；但是在 JavaScript 的世界中 `this` 是动态绑定的会让你摸不着头脑，经常会发现 `this` 并不是自己想要的`这个`。

我们把 `this` 的指向分成以下几种情况：

#### 外部函数调用
外部函数中调用 `this` 时，它将指向全局对象，浏览器中即为 `window`：

```
function whatIsThis() {
	console.log(this);
}
whatIsThis();
```
会打印出 `window` 对象

#### 内部函数调用
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

#### 对象的方法调用
对象的方法中调用`this`时，它将指向这个对象：

```
var obj = {
	a: 0,
	whatIsThis:() => {
		console.log(this);
	}
};
obj.whatIsThis();
```
会打印出当前`Object`对象

#### 构造函数内部调用
构造函数的对象调用`this`时，它将指向这个对象：

```
function Class() {
	console.log(this);
}
var obj = new Class();
```

#### 在`这里`总结一下 `this`
`this` 实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用：用对象调用的方法中使用的 `this` 会指向该对象；直接调用的函数中使用的 `this` 会指向全局对象 `window`。

#### 课后作业：`this` 是否能给它赋上另外一个对象呢？
在 JavaScript 中很多对象都可以进行扩展，包括它自带的内置对象，那么 `this` 可以吗？

### 邪恶(evil)的 `eval`
`eval` 很强大，它可以将字符串当作 JavaScript 运行，也就是说它能让代码在代码中运行；当然控制不好对于程序来说也是一个灾难，它会把你的作用域弄的非常混乱，很多 JavaScript 的编辑器的插件不允许使用 `eval` 这个关键字。

```
var b = 2;

function foo(str, a) {
	eval(str);
	console.log(a,b);
}

foo('var b = 3;', 1); //=>1, 3
```

```
function foo(str, a) {
	var b = 3；
	console.log(a,b);
}
```

阅读上面的代码，我们将字符串输入到 `foo` 方法中，最后导致了意想不到的结果，输入的内容动态的变成了代码的一部分，使得变量 `b` 在函数中的作用域产生了变化，并且屏蔽了外部作用域中的同名变量。这样会导致程序原本的逻辑产生变化，它就好像一个不讲道理的小孩，从不按套路出牌，让我们的程序难以控制，我们不建议使用它。最后，在程序中动态生成代码的使用场景非常罕见，因为它所带来的好处无法抵消性能上的损失。

### `with` or without 
`with` 这个语法也是一个比较难掌握的，和 `eval` 一样它会让程序的作用域非常混乱。它本身的作用是：当重复引用同一个对象中的多个属性时，可以使用这个快捷方式不需要重复引用对象本身。

```
var obj = {
	a: 1,
	b: 2,
	c: 3
};

// 重复的使用 obj
obj.a = 2;
obj.b = 3;
obj.c = 4;

// 快捷的使用 obj
with(obj) {
	a = 3;
	b = 4;
	c = 5;
}

```

看起来这种方式的确很好用，我们不用重复的去调用某个对象而对其中的属性一个一个赋值了，让开发更佳简便了。但是，糟糕的事情总是会在 JavaScript 中发生：

```
function foo(obj) {
	with(obj) {
		a = 2;
	}
}

var o1 = {
	a: 3
};

var o2 = {
	b: 3
};

foo(o1);
console.log(o1.a); // 2

foo(o2);
console.log(o2.a); // undefined
console.log(a);    // 2 —— ops! a 被泄露到全局作用域上了

```

结论：`with` 不能使用在一个该对象不存在的属性上，否则会将该属性变成一个全局对象！这种改变作用域的事情是一件非常恐怖的事情，不建议在实际项目中使用它；

### 可计算属性名
我们知道很多面相对象的语言中，可以通过 `.` 获取到某个对象下的某个属性，例如：`person.name`。当然这一点在 JavaScript 中完全没有问题，更佳恐怖的是属性名可以是变量，还可以传入表达式，我们叫做可计算属性名：

```
var person = {
	name: 'jay'
};

var propertyName = 'name';

console.log(person.name);
console.log(person['name']);
console.log(person[propertyName]);
console.log(person['na' + 'me']); // ES6
```

### 火眼金星看清一切的 `for/in`
`for/in` 这个语句可以遍历对象的属性，它就好像 `Java` 或 `C#` 中的反射机制一样，能够通过它窥探某个类型的基本结构，我们可以采用层层递归的方式，看清楚某个对象的所有属性。

```
function foo(obj) {
	for(key in obj) {
		console.log(obj[key]);
	}
}
```

### 将对象打回 `原型`
原型在 JavaScript 中是一个非常重要也非常难理解的知识点，搞清楚它可能需要一个专门的话题来讲它，现在我们只在这里简单的讨论。

`prototype` 和 `__proto__` 的关系相当复杂，`prototype` 为构造函数原型，`__proto__` 为对象的原型。所有构造函数的 `__proto__` 都指向 `Function.prototype`;`Function.prototype` 的原型指向 `Object.prototype`，而  `Object.prototype` 的原型为空 `null`，这样就构成了一条复杂的原型链。

```
var klass = function() {
	this.name = "jay";
}
var obj = new klass();

console.log(obj.__proto__ === klass.prototype);
console.log(klass.prototype.__proto__ === Object.prototype);
console.log(Object.prototype.__proto__ === null);

console.log(klass.constructor.__proto__ === Function.prototype);
console.log(Function.prototype.__proto__ === Object.prototype);
console.log(Object.prototype.__proto__ === null);

//> Console:
//> true
//> true
//> true
//> true
//> true
//> true
```

![JavaScript Prototype](resources/javascript-prototype.png)

```
Object.__proto__ === Function.__proto__
Function.__proto__ === Function.prototype
```

### 最后
工程师的工作非常枯燥，不过我们可以在枯燥的工作中找到一些有趣的事情。希望我们的工程师们能够善于发现工作中的乐趣，这样就会越来越充满激情。