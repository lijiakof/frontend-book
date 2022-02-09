# 什么鬼JS（wtfjs）

https://ironjs.wordpress.com/2011/06/22/my-gripes-with-javascript/
https://wtfjs.com/

JavaScript 是一种我们喜爱的编程语言，尽管给我们带来不少麻烦。我们收集了一系列反常规的 JavaScript 问题，包括一些在语言设计上的不一致、简单的、不直观的、令人痛苦的问题。

## 看起来差不多，但完全不一样

``` js
alert(111111111111111111111); // alerts 111111111111111110000
```

## foonanny
在计算`"foo" + (+ "bar")`时，将`"bar"`转化成了`NaN`（not a number，不是个数字）

// Proposal to rename this site: NaNwtf

``` js
("foo" + + "bar") === "fooNaN" // true
```

## function context fun
``` js
(x=[].reverse)() === window // true
```

// Thanks to @tobeytailor for pointing out this beauty.
这个错误是在Chrome的版本49中报出来的异常。

## maths fun
很显然，JavaScript 在数学计算中表现的并不是那么完美和直观。

``` js
typeof NaN === 'number' // true
Infinity === 1/0        // true
0.1 + 0.2 === 0.3       // false
```

## min number treachery
``` js
Number.MIN_VALUE > 0; // true? really? wtf.
```
结果表明，`MIN_VALUE`是一个大于`0`的最小数字，看起来完全有道理。

## not a number is a number
typeof NaN // number, of course.

Now that makes sense.
合乎情理

## not a number is not a not a number
Some argue this makes sense. Some ppl also like to sniff glue.

NaN === NaN // false

## null is not an object
    typeof null // object
    null instanceof Object // false
Classic null is not an Object.

## string is not string
    "string" instanceof String; // false. 
    // 'course it isn't not a string, it may look like a string
    // but actually it's masquerading as a banana.
When is a string, not a string? When it’s a duck - @rem

## accidental global
This one is fun and sneaky.

    (function(){
        var x = y = 1;
    })();
    alert(x); // undefined
    alert(y); // 1 -- oops, auto-global!
It’s treated like: var x = (y = 1); thus, “y=1” creates an auto-global since there’s no binding “var” statement for it. Afterwards, that value gets copied into the properly defined local var “x”.

