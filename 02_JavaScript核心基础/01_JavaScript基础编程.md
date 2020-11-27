<h1 id="JavaScript基础编程" align="center">JavaScript 基础编程</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. JavaScript 简介](#1-javascript-简介)
  - [1.1. 注释](#11-注释)
  - [1.2. 标识符与保留字](#12-标识符与保留字)
- [2. 变量与数据类型](#2-变量与数据类型)
  - [2.1. 变量声明](#21-变量声明)
  - [2.2. 数据类型](#22-数据类型)
  - [2.3. undefined 与 null](#23-undefined-与-null)
  - [2.4. boolean](#24-boolean)
  - [2.5. number 与 NaN](#25-number-与-nan)
  - [2.6. string](#26-string)
  - [2.7. number 与 string 之间的转换](#27-number-与-string-之间的转换)
  - [2.8. function](#28-function)
    - [2.8.1. 函数作为参数 和 拉姆达表达式](#281-函数作为参数-和-拉姆达表达式)
    - [2.8.2. 函数作为返回值](#282-函数作为返回值)
    - [2.8.3. 自调用函数(闭包)](#283-自调用函数闭包)
  - [2.9. 变量的作用域](#29-变量的作用域)
  - [2.10. 定义提前](#210-定义提前)
  - [2.11. array](#211-array)
    - [2.11.1. 基本 API 使用](#2111-基本-api-使用)
- [3. 运算符](#3-运算符)
  - [3.1. 相等比较 与 相同比较](#31-相等比较-与-相同比较)
- [4. 流程控制语句](#4-流程控制语句)
  - [4.1. for-in 可以输出数组下标 和 对象属性名](#41-for-in-可以输出数组下标-和-对象属性名)
  - [4.2. continue、break 和 lable](#42-continue-break-和-lable)
  - [4.3. with 语句](#43-with-语句)
  - [4.4. switch 语句](#44-switch-语句)
- [5. 面向对象与类](#5-面向对象与类)
  - [5.1. 对象字面量 json](#51-对象字面量-json)
  - [5.2. 动态对象](#52-动态对象)
  - [5.3. 类](#53-类)
- [6. ES6 新特性](#6-es6-新特性)
  - [6.1. let 和 const](#61-let-和-const)
  - [6.2. 解构赋值](#62-解构赋值)
  - [6.3. 扩展运算符与剩余运算符](#63-扩展运算符与剩余运算符)
  - [6.4. map() 和 reduce() 函数](#64-map-和-reduce-函数)
  - [6.5. 模块化](#65-模块化)

<!-- /code_chunk_output -->

# 1. JavaScript 简介

JavaScript 是一种轻量级的编程语言。
JavaScript 包括：核心语法（ECMAScript）、标签对象（DOM）、浏览器对象（BOM）。
JavaScript 严格区分大小写。

四种输出形式
- window.alert() 弹出警告框。
- document.write() 方法将内容写到 HTML 文档中。
- innerHTML 写入到 HTML 元素。
- console.log() 写入到浏览器的控制台。

## 1.1. 注释

```js
// 单行注释

/* 
    多行注释
    多行注释
*/

/**
 * 文档注释
 * @param {*} params 参数
 */
function name(params) {
    
}
```

## 1.2. 标识符与保留字

JavaScript 标识符必须以字母、下划线（_）或美元符（$）开始。
后续的字符可以是字母、数字、下划线或美元符。

|JavaScript保留字|>|>|>|
|-|-|-|-|
| abstract | else | instanceof | super |
| boolean | enum | int | switch |
| break | export | interface | synchronized |
| byte | extends | let | this |
| case | false | long | throw |
| catch | final | native | throws |
| char | finally | new | transient |
| class | float | null | true |
| const | for | package | try |
| continue | function | private | typeof |
| debugger | goto | protected | var |
| default | if | public | void |
| delete | implements | return | volatile |
| do | import | short | while |
| double | in | static | with |


# 2. 变量与数据类型

## 2.1. 变量声明

JavaScript 的变量拥有动态类型。这意味着相同的变量可用作不同的数据类型。

```js
var a;
```

## 2.2. 数据类型

![](https://atts.w3cschool.cn/attachments/image/20160809/1470710140220491.gif)

- 基本数据类型（值类型）：number、string、boolean、undefined、null、symbol
- 引用数据类型：object、array、function

注意：在 JavaScript 中 string 是基本数据类型。

```js
var a = 1;
console.log(a);
console.log(typeof(a));
a = 12.2;
console.log(a);
console.log(typeof(a));
// 1
// number
// 12.2
// number
a = "松松";
console.log(a);
console.log(typeof a);
// 松松
// string
a = true;
console.log(a);
console.log(typeof a);
// true
// boolean

a = new Date();
console.log(a);
console.log(typeof a);
// Wed Oct 07 2020 14:14:31 GMT+0800 (中国标准时间)
// object
a = [1,2,3];
console.log(a);
console.log(typeof a);
// [1, 2, 3]
// object
a = function f(){};
console.log(a);
console.log(typeof a);
// f(){}
// function
console.log("----------------");
```

## 2.3. undefined 与 null

- undefined 表示变量没有数据类型，且没有值。在一个变量被声明而未初始化时即为 undefined。
- null 表示变量的数据类型为 object，值为 null。
- undefined 派生于 null。因此 undefined == null 为 true

```js
console.log(undefined == null); // true
console.log(undefined === null); // false
```

```js
var b; // 声明了 类型是undefined
console.log(b);
console.log(typeof b);
// undefined
// undefined
b = 2;
console.log(b);
console.log(typeof b);
// 2
// number
b = null;
console.log(b);
console.log(typeof b);
// null
// object
b = undefined;
console.log(b);
console.log(typeof b);
// undefined
// undefined
console.log("----------------");
```

## 2.4. boolean

真 true；false 假

转换为 true：任何非空字符串，任何非零数值，任何非空对象。
转换为 false：空字符串，0和NaN，null和undefined。

```js
var str3 = "0"; // true
var str3 = 1; // true
var str3 = []; // true

var str3 = ''; // false
var str3 = ""; // false
var str3 = 0; // false
var str3 = NaN; // false
var str3 = null; // false
var str3; // false

if(str3) // 判断变量中是否有数据
{
    console.log("true");
}
else
{
    console.log("false");
}
```
谁能告诉我这是啥。。。
```js
console.log('' == false); // true
console.log("" == false); // true
console.log(0 == false); // true
console.log(NaN == false); // false
console.log(null == false); // false
console.log(undefined == false); // false

console.log(1 == true); // true
console.log("0" == true); // false
console.log([] == true); // false
```

## 2.5. number 与 NaN

number 包括整数和浮点数。

Number.MAX_VALUE = 1.7976931348623157e+308
Number.MIN_VALUE = 5e-324

NaN：Not a Number，非数值。
isNaN()：返回值：参数可转为数值，返回 true；否则，返回 false。
Number()：将参数转为数字。对于 `''、' '、'\t'、'\r'、'\n'、'0'、[]、[0]` 转化为 0；非数字、'\0'、undefined 转换为 NaN。

- NaN 与任何值不相等, 包括它本身
- 永远不要判断两个浮点数是否相等

```js
// 不要判断两个浮点数是否相等
var boo = (0.3 == 0.1 + 0.2);
console.log(0.3); // 0.3
console.log(0.1 + 0.2); // 0.30000000000000004
console.log(boo); // false
console.log("----------------");

// NaN 与任何值不相等, 包括它本身
console.log(isNaN("ww"));    // true   "ww" is a NaN
console.log(isNaN("10.9"));  // false  "10.9" is not a NaN
console.log(Number("ww"));   // NaN
console.log(Number("10.9")); // 10.9

console.log(NaN == Number("ww")); // false
console.log(NaN == Number("10.9")); // false
console.log(NaN == NaN); // false
console.log("----------------");
```

## 2.6. string

```js
var str = '<span style="color:red">\'hello\'</span>';
console.log(str);
console.log(str.length);
// <span style="color:red">'hello'</span>
// 38

console.log(11 + 11); // 22
console.log("11" + "11"); // 1111
console.log("11" + "11" + 11); // 111111
console.log(11 + 11 + "11"); // 2211
console.log(11 + "11" + 11 ); // 111111
```

## 2.7. number 与 string 之间的转换

number -> string：xxx.toString、String(xxx)
string -> number: Number(xxx)、parseInt(xxx)、parseFloat(xxx)

```js
var num = 19
console.log(typeof num); // number
num = num.toString();
console.log(typeof num); // string
num = Number(num);
console.log(typeof num); // number
var toStr = String(num);
console.log(typeof toStr); // string
console.log("----");

var str = "123abc";
console.log(parseInt(str)); // 123
console.log(parseFloat(str)); // 123
str = "abc123";
console.log(parseInt(str)); // NaN
console.log(parseFloat(str)); // NaN
str = "10.9abc123";
console.log(parseInt(str)); // 10
console.log(parseFloat(str)); // 10.9
console.log("----");

var s = '500';
console.log(typeof s); // string
console.log(+s); // 500
console.log(typeof (+s)); // number
console.log(-s); // -500
console.log(typeof (-s)); // number
console.log(s - 0); // 500
console.log(typeof (s - 0)); // number
console.log(s + 0); // 5000
console.log(typeof (s + 0)); // string
```


## 2.8. function

函数可以定义在任何位置。函数的调用可以在定义之前，因为浏览器会将函数定义提前。
如果没有在任何类中定义函数，则函数是定义在window对象中，很可能把window原有函数替换掉。同样，变量也是在window对象中。
没有函数重载，定义了多个同名函数，最终只有后定义的一个函数生效。

```js
demo();
function demo() {
    console.log("This is a fun");
}
demo();
window.demo();
// This is a fun
// This is a fun
// This is a fun
```


### 2.8.1. 函数作为参数 和 拉姆达表达式
```js
function runFun(fn)
{
    if(fn)
    {
        fn();
    }
}


function a() {
    console.log("我是传进去的函数");
}
runFun(a);

runFun(
    function () {
        console.log("我是传进去的匿名函数");
    }
);
// 拉姆达表达式
runFun(
    () => {
        console.log("我是传进去的拉姆达表达式");
    }
);
```

### 2.8.2. 函数作为返回值
```js
// 返回值
function ret() {
    return function name() {
        console.log("fun");
    };
}

var res = ret();
res();
// fun

ret()();
// fun
```

### 2.8.3. 自调用函数(闭包)
```js
(
    function c (a) 
    {
        console.log("自调用函数:"+a);
    }
)(12);

// 匿名
(
    function (a) 
    {
        console.log("自调用函数:"+a);
    }
)(12);
```

## 2.9. 变量的作用域

没有定义在任何类或者函数中的变量，都是全局变量，都会放到window对象中。
函数中定义的变量是局部变量，尽在当前打括号中有效。

## 2.10. 定义提前

变量和方法的声明会被提前
- 定义变量时, 变量的声明会被提升到作用域的最前面, 变量的赋值不会被提升。
- JavaScript解析器会把当前作用域中的函数声明提前到整个作用域的最前面。

```js
// 没有声明, 则报错
// console.log(A); // A is not defined

console.log(b); // undefined
var b; // 变量声明会提前

console.log(c); // undefined
var c = 1; // 但是赋值不会提前
console.log(c); // 1
```

```js
// 以下三种：一个有赋值, 一个没有赋值
// 方法会覆盖变量
// 因为方法和变量同时都会提前
// 变量只是提前声明, 不会提前赋值
// 方法提前声明的同时也就相当于定义了方法体
console.log(typeof fun0); // function
var fun0 = 12; // 赋值, 类型变为number
function fun0() {}
console.log(typeof fun0); // number

console.log(typeof fun1); // function
var fun1;
function fun1() {}
console.log(typeof fun1); // function

console.log(typeof fun2); // function
function fun2() {}
var fun2;
console.log(typeof fun2); // function
```

## 2.11. array

JavaScript 的数组相当于 集合 队列 栈 数组 的结合体，可以存任意类型的元素。

```js
// 创建数组的两种方式
var arr1 = [];
console.log(arr1); // []

var arr2 = [1,2,3,4];
console.log(arr2); // [1,2,3,4]
console.log(arr2[3]); // 4，下标从 0 开始

var arr3 = [1,'123',2.3,new Date(), function(){ alert("松松"); }];
console.log(arr3); 
// [1, "123", 2.3, Wed Oct 07 2020 15:59:07 GMT+0800 (中国标准时间), ƒ]
console.log(arr3.length); // 5
arr3[4](); // 调用方法
console.log("------------");
```

### 2.11.1. 基本 API 使用

```js
(function(){
    var arr = [];
    arr.push(1); // 入栈 放到最后面
    arr.push(2);
    console.log(arr);
    arr.push(3);
    console.log(arr);
    arr.splice(1, 2); // 从下标1处开始往后删除2个元素
    console.log(arr);
    arr.push(4);
    console.log(arr);
    // 从下标1处开始往后删除0个元素
    // 然后在下标1处在插入一个 5
    arr.splice(1, 0, 5); 
    console.log(arr);
})();
console.log("------------");

console.log("栈");
(function(){
    var arr = [];
    arr.push(1);
    arr.push(2);
    arr.push(3);
    console.log(arr);       // [1, 2, 3]
    console.log(arr.pop()); // 3
    console.log(arr);       // [1, 2]
})();

console.log("队列");
(function(){
    var arr = [];
    arr.push(1);
    arr.push(2);
    arr.push(3);
    console.log(arr);           // [1, 2, 3]
    console.log(arr.shift());   // 1
    console.log(arr);           // [2, 3]
    console.log(arr.shift());   // 2
    console.log(arr);           // [3]
})();
```


# 3. 运算符

## 3.1. 相等比较 与 相同比较

== : 相等比较, 判断值是否相等, 两边的操作数会发生类型转换。
===: 相同比较, 判断值和类型是否都相同, 如果类型不相同则为 false。
```js
console.log(12 == "12"); // true 
console.log(12 === "12"); // false
```


```js
// 这是啥。。。
console.log(0 == "\t");
console.log(0 == []);
console.log(0 == "0");
console.log(0 == "");
console.log([] == "");
console.log("--")
// 以上均为 true

console.log(0 == "\0");
console.log("\t" == []);
console.log("\t" == "0");
console.log("\t" == "");
console.log([] == "0");
console.log("0" == "");
console.log(NaN == NaN);
console.log("--")
// 以上均为 false

// 还有下面这个。。。
console.log('' == false); // true
console.log("" == false); // true
console.log(0 == false); // true
console.log(NaN == false); // false
console.log(null == false); // false
console.log(undefined == false); // false

console.log(1 == true); // true
console.log("0" == true); // false
console.log([] == true); // false
```

# 4. 流程控制语句

JavaScript 的语句以分号结尾，也可以省略分号。

## 4.1. for-in 可以输出数组下标 和 对象属性名
```js
for (var key in arr) {
    console.log(key, arr[key]);
}
console.log("--");
var str ="松松";
for (var key in str) {
    console.log(key, str[key]);
}
// 0 11
// 1 22
// 2 31
// 3 42
// --
// 0 松
// 1 松
```

## 4.2. continue、break 和 lable
break 结束外层循环：
```js
A : for(let i = 0 ; i < 10; i++) {
    for(let j = 0; j < 10; j++) {
        console.log("j="+j);
        if(j == 2) {
            break A;
        }
    }
    console.log("i="+i);
}
// j=0
// j=1
// j=2
```

## 4.3. with 语句
将代码的作用域设定到一个特定对象中。
```js
// var qs = location.search.substring(1);
// var name = location.hostname;
// var url = location.href;
// console.log(qs, name, url)

with (location) {
    var qs = search.substring(1);
    var name = hostname;
    var url = href;
    console.log(qs, name, url)
}
```

## 4.4. switch 语句

JavaScript 的 switch 语句中，case 的值可以是常量、变量、表达式。
```js
var num = 15;
switch (true) {
    case num < 0:
        console.log("Less than 0.");
        break;
    case num >= 0 && num <= 10:
        console.log("Between 0 and 10.");
        break;
    case num > 10 && num <= 20:
        console.log("Between 10 and 20.");
        break;
    default:
        console.log("More than 20.");
}
// Between 10 and 20.
```


# 5. 面向对象与类

## 5.1. 对象字面量 json

```js
// 对象字面量 json对象: 用来传递数据 一次性的
var student = {
    id: 1,
    name: "松松",

    show: function () {
        // 必须要 this
        console.log("id: "+this.id + "  name: "+this.name);
    }
};
student.show(); // id: 1  name: 松松
console.log(student.name); // 松松

for (key in student) {
    console.log(key, student[key]);
}
// id 1
// name 松松
// show ƒ () {
//     // 必须要 this
//     console.log("id: "+this.id + "  name: "+this.name);
// }
```

json 数组

```js
(function () {
    var students = [
        {id: 1, name: "松松"},
        {id: 2, name: "剑剑"},
        {id: 3, name: "宁宁"},
        {id: 4, name: "志斌"},
        {id: 5, name: "小丁"}
    ];

    for (let i = 0; i < students.length; i++) {
        const element = students[i];
        console.log(element.name);
    }
})();
// 松松
// 剑剑
// 宁宁
// 志斌
// 小丁
```

## 5.2. 动态对象

JavaScript 中对象的属性是可以动态添加的

```js
var obj = new Object();
obj.name = "松松"; // 没有这个属性就会定义这个属性
obj.show = function () {
    console.log(this.name);
}
obj.show(); // 松松

function demo() {
    // this 指当前对象
    console.log(this);
}
demo(); // 添加到window对象的属性
```

## 5.3. 类

删除对象的属性 `delete stu1.name`

```js
// 自定义构造函数
// 通过名字区分普通方法和类
// 类首字母大写, 方法小写
function Student(id, name, age){
    this.id = id;
    this.name = name;
    this.age = age;

    this.show = function () {
        console.log(
            "id: " + this.id + "  " +
            "name: " + this.name + "  " +
            "age: " + this.age
        );
    }
}

var stu1 = new Student(1, "松松", 20);
var stu2 = new Student(2, "宁宁", 13);
stu1.show();
stu2.show();
// id: 1  name: 松松  age: 20
// id: 2  name: 宁宁  age: 13
```

```js
// 再加上闭包, 防止污染window
(function () {
    function Student(id, name, age){
        this.id = id;
        this.name = name;
        this.age = age;
    
        this.show = function () {
            console.log(
                "id: " + this.id + "  " +
                "name: " + this.name + "  " +
                "age: " + this.age
            );
        }
    }
    
    var stu1 = new Student(1, "松松", 20);
    var stu2 = new Student(2, "宁宁", 13);
    stu1.show();
    stu2.show();

    // for-in 对象的属性 反射
    for (var key in stu1) {
        console.log(key);
    }
    // 通过中括号调用属性
    stu1["show"]();

    // 删除对象的属性
    delete stu1.name

    // 遍历对象的属性, 不要方法
    for (var key in stu1) {
        if((typeof stu1[key]) != "function")
        {
            console.log(key + ": " +stu1[key]);
        }
    }

    console.log("----------------");
})();

// id: 1  name: 松松  age: 20
// id: 2  name: 宁宁  age: 13
// id
// name
// age
// show
// id: 1  name: 松松  age: 20
// id: 1
// age: 20
// ----------------
```


# 6. ES6 新特性

ECMAScript 6 于 2015 年 6 月发布。

## 6.1. let 和 const

使用 var 声明的变量无法控制其作用域。
```js
for (var i = 0; i < 5; i++) {
    var j = 10;
}
console.log("循环外:", i); // 循环外: 5
console.log("循环外:", j); // 循环外: 10
```

而使用 let 声明的变量只会在当前作用域内存在。
- let 是在代码块内有效，var 是在全局范围内有效。
- let 只能声明一次 var 可以声明多次。
- let 不存在变量提升，var 会变量提升。
```js
for (let i = 0; i < 5; i++) {
    let j = 10;
}
console.log("循环外:", i); // Uncaught ReferenceError: i is not defined
console.log("循环外:", j); // Uncaught ReferenceError: j is not defined

var b = 3;
var b = 4;
console.log(b); // 4
let a = 1;
let a = 2; // Uncaught SyntaxError: Identifier 'a' has already been declared

console.log(b); // undefined
var b = "banana";
console.log(a); // Uncaught ReferenceError: Cannot access 'a' before initialization
let a = "apple";
```


const 用于声明只读变量，只能被赋值一次，无法修改。必须在声明时初始化。
```js
const a = 12;
a = 10; // Uncaught TypeError: Assignment to constant variable.

const b; // Uncaught SyntaxError: Missing initializer in const declaration
```

## 6.2. 解构赋值

**数组解构**
```js
var arr = [1, 2, 3];

// 解构数组
var [x, y, z] = arr;
console.log(x, y, z); // 1 2 3
// 忽略末尾
var [a, b] = arr;
console.log(a, b); // 1 2
// 忽略中间部分
var [c, , d] = arr;
console.log(c, d); // 1 3
// 不完全解构、解构默认值
var [e, f, g=0, h, i=1] = arr;
console.log(e, f, g, h, i); // 1 2 3 undefined 1

// 解构字符串
let [a, b, c, d, e] = 'hello';
console.log(a, b, c, d, e); // h e l l o

// 可嵌套使用
var s = 'xyz';
var arr = [1, s, 2];
let [a, [x, y, z], c] = arr;
console.log(a, x, y, z, c); // 1 "x" "y" "z" 2
```

**对象解构**
```js
const person = {
    name: "jack",
    age: 21,
    language: ['html', 'css', 'js']
};

let {name, age, language} = person;
console.log(name); // jack
console.log(age);  // 21
console.log(language); // ['html', 'css', 'js']

let {name:n} = person;
console.log(n); // jack
```


## 6.3. 扩展运算符与剩余运算符

扩展运算符：将一个数组或者对象的所有元素展开为一个逗号分隔的参数序列。
```js
console.log([1, 2, 3]);       // [1, 2, 3]
console.log(...[1, 2, 3]);    // 1 2 3
console.log([1, 2, 3], 4);    // [1, 2, 3] 4
console.log(...[1, 2, 3], 4); // 1 2 3 4

// 函数入参
console.log(
    (
        function add(a, b) {
            return a + b;
        }
    )(...[1, 2])
); // 3

// 合并数组
var a = [1, 2, 3];
var b = [4, 5, 6];
console.log([...a, ...b]); // [1, 2, 3, 4, 5, 6]

console.log([..."hello"]); // ["h", "e", "l", "l", "o"]
```

扩展运算符会展开数组变成多个元素，剩余运算符会收集多个元素和压缩成一个单一的元素。
```js
// 剩余运算符
let [a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]

(
    function add(a, ...b) {
        console.log(a); // 1
        console.log(b); // [2, 3]
    }
)(1, 2, 3)
```

## 6.4. map() 和 reduce() 函数

`let newArray = array.map(function)`: 接收一个函数 function，将原数组 array 中的所有元素用这个函数 function 处理后放入新数组 newArray 返回。
函数 function 有一个参数: 数组中当前元素
```js
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(numbers); // [1,2,3,4,5]
console.log(doubled); // [2,4,6,8,10]
```

reduce()∶接收一个函数 function(必须)和一个初始值 value (可选)。
函数 function 有两个参数:
- 第一个参数：上一次 reduce() 处理的结果
- 第二个参数：数组中要处理的下一个元素

reduce() 会从左到右依次把数组中的元素用reduce处理，并把处理的结果作为下次reduce的第一个参数。
如果是第一次reduce，没有指定初始值时，会把数组前两个元素作为参数；如果指定的初始值，则初始值和数组第一个元素作为参数。
```js
// 没有指定初始值
let arr = [1, 2, 3, 4];
arr.reduce(
    (a, b) => {
        console.log(a, b, a+b);
        return a + b;
    }
);
// a b 结果
// 1 2 3
// 3 3 6
// 6 4 10

// 指定初始值为 10
let arr = [1, 2, 3, 4];
arr.reduce(
    (a, b) => {
        console.log(a, b, a+b);
        return a + b;
    },
    10
);
//  a b 结果
// 10 1 11
// 11 2 13
// 13 3 16
// 16 4 20
```

## 6.5. 模块化

模块化就是把代码进行拆分，方便重复利用（类似java中的导包: 要使用一个包，必须先导包）。、
而 JS 中没有包的概念，换来的是模块。 
> export: 规定模块的对外接口，
> import: 导入其他模块提供的功能。

- 导出的函数声明与类声明必须要有名称
- 不仅能导出声明还能导出引用（例如函数）。
- export 命令可以出现在模块的任何位置，但必需处于模块顶层。
- import 命令会提升到整个模块的头部，首先执行。

- 在一个文件或模块中，export、import 可以有多个，export default 仅有一个。
- export default 中的 default 是对应的导出接口变量。
- 通过 export 方式导出，在导入时要加{ }，export default 则不需要。
- export default 向外暴露的成员，可以使用任意变量来接收。

在文件夹 module 下创建三个文件 package.json、Util.js、index.js
package.json（"type": "module" 这一项必须）
```json
{
  "name": "module",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
Util.js
```js
// 定义一个类
class Util {
    // 其中有一个静态方法
    static sum = (a, b) => a + b;
}
// 导出该类
export default Util;
```
index.js
```js
import Util from './Util.js'

console.log(Util.sum(1, 1));
```

运行 Node
```shell
> node ./index.js
2
```