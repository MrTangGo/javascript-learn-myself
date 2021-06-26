
# 类型检测

## typeof 

可以使用typeof用于判断数据的类型
```js
let a = 1;
console.log(typeof a); 
//number

let b = "1";
console.log(typeof b); 
//string

//未赋值或不存在的变量返回undefined
var gg;
console.log(typeof gg);
// undefined

function run() {}
console.log(typeof run); 
//function

let c = [1, 2, 3]; // 数组
console.log(typeof c); 
//object

let d = { name: "houdunren.com" }; // 对象
console.log(typeof d);
//object
```

## instanceof

`instanceof` 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

也可以理解为是否为某个对象的实例，`typeof`不能区分数组，但`instanceof`则可以。

```js
let a = [];
let b = {};
console.log(a instanceof Array); 
//true
console.log(b instanceof Array); 
//false

let c = [1, 2, 3];
console.log(c instanceof Array); 
//true

let d = { name: "houdunren.com" };
console.log(d instanceof Object); 
//true

function User() {}
let f = new User();
console.log(f instanceof User);
 //true
```

## 值类型与对象

下面是使用字面量与对象方法创建字符串，返回的是不同类型。
```js
let a = "hello";
let b = new String("world");

console.log(typeof a);
// string
console.log(typeof b);
// object
```

只有对象才有方法使用，但在JS中也可以使用值类型调用方法，因为它会在执行时将值类型转为对象。
```js
let a = "hello";
let b = new String("Tom");
console.log(a.length);  // 在这里，a会在执行时将值类型转为对象
//5
console.log(b.length); 
//3
```






# 字符串

## 双引号与单引号
字符串值类型，如果想要在里面打出双引号，需要加上转义符。或者交替使用大小双引号。
```js
let a = "she said:\"this is a book.\"";
console.log(a);
console.log("she said:'this is a book.'");
console.log('she said:"this is a book."');
```

## 格式化输出/模板字面量使用
常用的方式
```js
// 格式化输出
let year = "2021"
let month = "5"
let day = "20"
console.log(`${year}年${month}月${day}日`);
//2021年5月20日
```

模板中可以使用函数或者表达式
```js
// 模板字面量的使用
function show() {
    return "baidu"
};
console.log(`www.${show()}.com`);

console.log(`${5+1}`);

// www.baidu.com
// 6
```

例子
```js
let lesson = [
    {title: "aaa"},
    {title: "bbb"},
    {title: "ccc"},
]
function template() {
    return `<ul>
        ${lesson.map(item => `<li>${item.title}</li>` ).join("")}
        </ul>`
};
document.body.innerHTML = template();
```

## 标签模板
标签模板是提取出普通字符串与变量，交由标签函数处理。
```js
let year = 2021;
let month = 5;
let day = 20;

tag `${year}年 ${month}月 ${day} 日`;

function tag(str, ...vars) {
    console.log(str);// ['年', '月', '日']
    console.log(vars);// [2021, 5, 20]
};
// 
```

例子：标题中有等等的使用标签模板加上链接
```js
let lessons = [{
        title: "线性代数",
        author: "等等"
    },
    {
        title: "微积分",
        author: "刘老师"
    },
    {
        title: "概率论",
        author: "等等"
    },
    {
        title: "基础数学",
        author: "刘老师"
    },
];

function template() {
    return `<ul>
            ${lessons.map((item, key) => 
                link`<ul>作者：${item.author} 标题：${item.title}</ul>`
            ).join("")}
        <ul>`
}

function link(str, ...vars) {
    return str.map((item, key) => item +
        (vars[key] ? vars[key].replace(
                '等等',
                "<a href='https://www.baidu.com'>等等</a>") :
            '')).join("")
}
document.body.innerHTML += template();
```

