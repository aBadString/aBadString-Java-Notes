<h1 id="JavaScript基础编程" align="center">JavaScript 基础编程</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. JavaScript 简介](#1-javascript-简介)
  - [1.1. 注释](#11-注释)
- [2. 变量与数据类型](#2-变量与数据类型)
  - [2.1. 变量声明](#21-变量声明)
  - [2.2. 数据类型](#22-数据类型)
  - [2.3. undefined 与 null](#23-undefined-与-null)
  - [2.4. boolean](#24-boolean)
  - [2.5. number 与 NaN](#25-number-与-nan)
  - [2.6. string](#26-string)
  - [2.7. number 与 string 之间的转换](#27-number-与-string-之间的转换)
  - [2.8. array](#28-array)
    - [2.8.1. 基本 API 使用](#281-基本-api-使用)
  - [2.9. function](#29-function)
    - [2.9.1. 函数作为参数 和 拉姆达表达式](#291-函数作为参数-和-拉姆达表达式)
    - [2.9.2. 函数作为返回值](#292-函数作为返回值)
    - [2.9.3. 自调用函数(闭包)](#293-自调用函数闭包)
  - [2.10. 变量的作用域](#210-变量的作用域)
  - [2.11. 定义提前](#211-定义提前)
- [3. 标识符与保留字](#3-标识符与保留字)
- [4. 运算符](#4-运算符)
  - [4.1. 相等比较 与 相同比较](#41-相等比较-与-相同比较)
- [5. 流程控制语句](#5-流程控制语句)
  - [5.1. for-in 可以输出数组下标 和 对象属性名](#51-for-in-可以输出数组下标-和-对象属性名)
- [6. 面向对象与类](#6-面向对象与类)
  - [6.1. 对象字面量 json](#61-对象字面量-json)
  - [6.2. 动态对象](#62-动态对象)
  - [6.3. 类](#63-类)

<!-- /code_chunk_output -->

# 1. JavaScript 简介

JavaScript 是一种轻量级的编程语言。
JavaScript 包括：核心语法（ECMAScript）、标签对象（DOM）、浏览器对象（BOM）

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


# 2. 变量与数据类型

## 2.1. 变量声明

JavaScript 的变量拥有动态类型。这意味着相同的变量可用作不同的数据类型。

```js
var a;
```

## 2.2. 数据类型

![](https://atts.w3cschool.cn/attachments/image/20160809/1470710140220491.gif)

 - 基本数据类型（值类型）：number、string、boolean、undefined、null、Symbol
 - 引用数据类型：object、array、function

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
Number()：将参数转为数字。对于 `''、' '、'\t'、'\r'、'\n'、'0'、[]、[0]` 转化为 0；非数字、'\0'转换为 NaN。

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

## 2.8. array

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

### 2.8.1. 基本 API 使用

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

## 2.9. function

函数可以定义在任何位置。函数的调用可以在定义之前，因为浏览器会将函数定义提前。

如果没有在任何类中定义函数，则函数是定义在window对象中，很可能把window原有函数替换掉。同样，变量也是在window对象中。

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


### 2.9.1. 函数作为参数 和 拉姆达表达式
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

### 2.9.2. 函数作为返回值
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

### 2.9.3. 自调用函数(闭包)
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

## 2.10. 变量的作用域

没有定义在任何类或者函数中的变量，都是全局变量，都会放到window对象中。
函数中定义的变量是局部变量，尽在当前打括号中有效。

## 2.11. 定义提前

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

# 3. 标识符与保留字

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


# 4. 运算符

## 4.1. 相等比较 与 相同比较

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

# 5. 流程控制语句

## 5.1. for-in 可以输出数组下标 和 对象属性名
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

# 6. 面向对象与类

## 6.1. 对象字面量 json

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

## 6.2. 动态对象

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

## 6.3. 类

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