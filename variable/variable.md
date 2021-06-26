# 变量

## 命名规则

JS中的变量是弱类型的，可以保存所有类型的数据，即变量没有类型而值有类型。变量名以字母、$ 美元符、_ 下划线 开始，后跟字母、数字、_ 下划线。
```js
let name = 'Hello World';
let $='Hello World'
var _a = 'Hello'
```

JS语言关键字不能用来做变量名，比如 true、if、while、class 等。
```js
let class = 'houdunren';
// error 不能用class关键字作为变量名
```

name在浏览器里面是一个有值的变量，可能会产生歧义。
```js
console.log(name)
// 99
```

## 弱类型

弱类型的意思是，变量的类型会根据值的类型而改变。
```js
var a = "hello";
console.log(typeof a);
a = 66;
console.log(typeof a);
a = {}
console.log(typeof a);
//string
//number
//object
```

## 变量声明

可以使用多种方式定义变量比如var、let等。
变量的声明与赋值是两个过程
```js
let name ; \\声明
name = 'Tom' ;\\赋值
```

结合起来就是：
```js
let name = 'Tom';
```

使用-,- 可以同时声明多个变量
```js
let n = 2,f = 3;
console.log(f);
```

## 变量提升
在使用var声明时，会遇到变量提升现象。
```js
var web = 'hello';
console.log(web);
var while = 'world'; 
```
按照其他语言的思路上面的代码可能会输出为:
```js
//hello
//Uncaught SyntaxError: Unexpected token 'while'
```
但是代码在解析过程中发现while不能做为变量名，没有到执行环节就出错了。
最后的输出为：
```js
//Uncaught SyntaxError: Unexpected token 'while'
```

这是因为解析器会先解析代码，然后把声明的变量的声明提升到最前。这个过程就叫做变量提升。
上面的代码在解释器中会变成：
```js
var web;
var while;
web = 'hello';
console.log(web);
while = 'world'; 
//Uncaught SyntaxError: Unexpected token 'while'
```

例一：
```js
console.log(web);
var web = 'hello'
// undefine
```
实际的解析过程是：
```js
var web;
console.log(web);
web = 'hello'
```

例二：

```js
// 变量提升
function test() {
    if (false) {
        var a = 'hello';
    }
    console.log(a);
}
test();

// undefined
```

实际的解析过程是：
```js
// 变量提升
function test() {
    var a;
    if (false) {
        a = 'hello';
    }
    console.log(a);
}
test();

// undefined
```

## TDZ

变量提升的现象在let声明中不起作用。变量要在let要在声明之后使用，不能在声明之前使用。
```js
console.log(a)
let a = 'Tom'
// Uncaught ReferenceError: Cannot access 'a' before initialization
```
```js
console.log(a)
const a = 'Tom'
// Uncaught ReferenceError: Cannot access 'a' before initialization
```

```js
test = "hello";
function run() {
  console.log(test);
  let test = "tom";
}
run();
//Uncaught ReferenceError: Cannot access 'test' before initialization
```

## 全局污染

如果不用var/let声明变量，就会照成全局污染。

```js
//1.js
function show() {
    web = 'world'
}
```

```html
<script src="1.js"></script>
<script>
    web = 'hello'
    show();
    console.log(web)
</script>
//world
```

解决办法：严格模式 "use strict";
```js
"use strict";
web = 'hello'
show();
console.log(web)
//Uncaught ReferenceError: web is not defined at
```


## 块作用域
### 共同点

var/let/const共同点是全局作用域中定义的变量，可以在函数中使用。

```js
var test1 = 'test1';
function show1() {
    return test1;
}
console.log(show1());

let test2 = 'test2';
function show2() {
    return test2;
}
console.log(show2());

const test3 = 'test3';
function show3() {
    return test3;
}
console.log(show3());

//test1
//test2
//test3
```

函数中声明的变量，只能在函数及其子函数中使用。除非外部有定义，否则无法使用。
```js
function test() {
  var web = "hello";

  function innerTest() {
    console.log(web);
  }
  innerTest(); //子函数结果: hello
  console.log(web); 
}
test();//函数结果: hello
console.log(web); //全局访问: web is not defined
```

```js
var web = "testOuter";
function test() {
  var web = "testInner";
  console.log(web); 
}
test();//testInner
console.log(web); //testOuter
```



### var
var会污染全局.
```js
var i = 99
for (var i = 0; i < 10; i++) {
  console.log(i);
}
console.log(i);
// 10
```

使用let有块作用域时则不会.
```js
let i = 100;
for (let i = 0; i < 6; i++) {
  console.log(i);
}
console.log(i);
// 100
```

### const

const声明的是一个常量，是不能改变的。为了好区分，常量名一般用大写的字母表示。

常量声明时必须同时赋值。
```js
const URL;
URL = "https://www.google.com"; 
//Uncaught SyntaxError: Missing initializer in const declaration
```

常量不允许全新赋值.
```js
const URL = "https://www.baidu.com";
URL = "https://www.google.com"; 
//Uncaught TypeError: Assignment to constant variable.
```
```js
const OBJA = {'lily':1};
OBJA = {
    'tom': 1
};
//Uncaught TypeError: Assignment to constant variable
```

但是，改变常量的引用类型值是可以的。因为常量指向引用类型的地址没有发生变化。
```js
const INFO = {
    url: 'https://www.baidu.com',
    port: '8080'
};
INFO.port = '443';
console.log(INFO);
// {url: "https://www.baidu.com", port: "443"}
```

不同作用域中可以重名定义常量.
```js
const NAME = 'baidu';
function show() {
  const NAME = 'google';
  return NAME;
}
console.log(show());
console.log(NAME);
```

总结：
- 常量名建议全部大写
- 只能声明一次变量
- 声明时必须同时赋值且不允许再次全新赋值
- 可以修改引用类型变量的值
- 拥有块、函数、全局作用域


## window全局对象污染

> Window 对象
> 所有浏览器都支持 window 对象。它代表浏览器的窗口。

在使用var声明一个变量的时候会把变量挂载到window上面。
```js
var web = 'hello'
console.log(window.web)
```
这样的声明容易给window对象照成污染。
举一个例子。
```js
// window全局污染
var screenLeft = 10;
console.log(window.screenLeft);
// 10
```
其实`window.screenLeft`表示的是浏览器到屏幕左边的距离。但是，在`var`声明中，如果正好有一个变量名也叫`window.screenLeft`，就会照成输出的错误。也是就说变量的声明给window对象照成了污染。

解决的方法：
```js
// 用let声明解决window全局污染
let screenLeft = 10;
console.log(window.screenLeft);
console.log(screenLeft);
// 根据实际的左边距输出
// 10
```
也就是说`let`声明赋值的变量，不会被挂载到`window`上面。
```js
let web = 'hello'
console.log(window.web)
// undefined
```

## 重复定义
在同一个作用域下面，使用`var`定义了同名变量，是不会报错的。
```js
//优惠价
var price = 90;
//商品价格
var price = 100;
console.log(`商品优惠价格是:${price}`);
// 商品优惠价格是:100
```

在同一个作用域下面，使用`let/const`定义的时候会报错。
```js
//优惠价
let price = 90;
//商品价格
let price = 100;
console.log(`商品优惠价格是:${price}`);
// Uncaught SyntaxError: Identifier 'price' has already been declared
```

## Object.freeze冻结变量
前面说过，可以改变引用类型常量中的值，因为引用类型的地址没有发生变化。
```js
const HOST = {
  url: "www.baidu.com",
  port: 8888
};
HOST.port = 6666;
console.log(HOST)
```

如果想要连引用类型中的值都不要发生变化的话，可以用`Object.freeze`冻结。
```js
const HOST = {
    url: "www.baidu.com",
    port: 8888
};
HOST.port = 6666;
console.log(HOST)

Object.freeze(HOST)
HOST.port = 5555;
console.log(HOST)
// {url: "www.baidu.com", port: 6666}
// {url: "www.baidu.com", port: 6666}
```

如果代码的前面有`"use strict"`就会在语法的层面上阻止这样的修改。
```js
// {url: "www.baidu.com", port: 6666}
// Uncaught TypeError: Cannot assign to read only property 'port' of object '#<Object>'
```

## 传值与传址
基本数据类型指数值、字符串等简单数据类型，引用类型指对象数据类型。

基本类型复制是值的复制，互相不受影响。下例中将a变量的值赋值给b变量后，因为基本类型变量是独立的所以a的改变不会影响b变量的值。
```js
let a = 100;
let b = a;
a = 200;
console.log(b);
console.log(a);
// 100
// 200
```

对于引用类型来讲，变量保存的是引用对象的指针。变量间赋值时其实赋值是变量的指针，这样多个变量就引用的是同一个对象。
```js
let a = {
  web: "baidu"
};
let b = a;
a.web = "google";
console.log(b);
console.log(a);
// {web: "google"}
// {web: "google"}
```

## undefine

对声明但未赋值的变量, 输出的值为`undefined`，判断类型将显示为`undefined`, 表示值未定义, 但不报错。

```js
let a;
console.log(a);
console.log(typeof a);
// undefine
// undefine
```

对未声明的变量，输出的值为`错误：Uncaught ReferenceError: a is not defined` ， 判断类型将显示为`undefined`。

```js
console.log(a);
// Uncaught ReferenceError: a is not defined
```
```js
console.log(typeof a);
// undefined
```

函数参数或无返回值是为`undefined`.
```js
function test(web) {
    console.log(web); //undefined
    return web;
}
console.log(test()); //undefined
```

## null 
`null`用于定义一个空对象，即如果变量要用来保存引用类型，可以在初始化时将其设置为`null`.

```js
var test = null;
console.log(typeof test);
console.log(test);
// object 
// null
```

## 严格模式： “use strict”
严格模式可以让我们及早发现错误，使代码更安全规范，推荐在代码中一直保持严格模式运行。

变量必须使用关键词声明，未声明的变量不允许赋值。
```js
"use strict";
url = 'houdunren.com'; 
//Uncaught ReferenceError: url is not defined
```

强制声明防止污染全局。
```js
"use strict";
function test() {
    web = "baidu";
}
test();
console.log(web); 
// Uncaught ReferenceError: web is not defined
```

关键词不允许做变量使用
```js
"use strict";
var public = 'baidu';
// Uncaught SyntaxError: Unexpected strict mode reserved word
```

变量参数不允许重复定义.
```js
"use strict";
//不允许参数重名
function test(name, name) {} 
// Uncaught SyntaxError: Duplicate parameter name not allowed in this context
```











