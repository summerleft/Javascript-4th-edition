# Chapter03 语言基础

本章内容主要基于ECMAScript第6版

## 3.1 语法

### 3.1.1 区分大小写

### 3.2.2 标识符

* 第一个字符必须是字母、下划线或美元符号
* 其他字符可以是字母、下划线、美元符号或数字
* 字母可以是扩展ASCII（Extended ASCII）中的字母，也可以是Unicode字符
* 按照惯例，使用驼峰大小写形式

### 3.1.3 注释

```javascript
// 单行注释
/* 这是多行
注释 */
```

### 3.1.4 严格模式

ECMAScript 3的一些不规范写法在这种模式下会被处理，对于不安全的活动将抛出错误。启用严格模式，在脚本靠头加上这一行：

```javascript
"use strict";
```

也可以单独指定一个函数在严格模式下执行

```javascript
function doSomething() {
  "use strict";
  // 函数体
}
```

### 3.1.5 语句

ECMAScript中的语句以分号结尾，省略分号泽由解析器确定语句在哪里结尾

## 3.2 关键字与保留字

## 3.3 变量

ECMAScript变量是松散类型，即变量可以用于保存任何类型的值

var在ECMAScript所有版本都可以使用，const和let只能在ECMAScript6及更晚的版本使用

### 3.3.1 var关键字

```javascript
var message;
```

若不进行初始化，变量会保存一个特殊值undefined

不仅可以改变保存的值，也可以改变值的类型

```javascript
var message = "hi";
message = 10; // 合法，但不推荐
```

#### 3.3.1.1 var声明作用域

var操作符定义的变量会成为包含它的函数的局部变量

```javascript
function test() {
  var message = "hi"; // 局部变量
}
test();
console.log(message); // 出错！
```

在函数体内定义变量时省略var操作符，可以创建全局变量

```javascript
function test() {
  message = "hi"; // 全局变量
}
test();
console.log(message); // "hi"
```

> 不推荐通过省略var定义全局变量。严格模式下，给未声明的变量赋值会导致抛出ReferenceError

定义多个变量可以在一条语句中用逗号分隔每个变量（及可选的初始化）

```javascript
var message = "hi",
    found = false,
    age = 29;
```

#### 3.3.1.2 var声明提升

使用var声明的变量会自动提升到函数作用域顶部

```javascript
function foo() {
  console.log(age);
  var age = 26;
}
foo(); // undefined
```

上述代码不会报错，ECMAScript运行时把它等价于如下代码

```javascript
function foo() {
  var age;
  console.log(age);
  age = 26;
}
foo(); // undefined
```

也可反复多次使用var声明同一个变量

```javascript
function foo() {
  var age = 16;
  var age = 26;
  var age = 36;
  console.log(age);
}
foo(); // 36
```

### 3.3.2 let声明

let和var相近，但let声明的范围是块作用域，var声明的范围是函数作用域

```javascript
if (true) {
  var name = 'Matt';
  console.log(name); // Matt
}
console.log(name); // Matt

if (true) {
  let age = 26;
  console.log(age); // 26
}
console.log(age); // ReferenceError: age没有定义
```

let不允许同一个块中出现冗余声明

```javascript
var name;
var name;

let age;
let age; // SyntaxError: 标识符age已经声明过了
```

嵌套使用相同的标识符不会报错，因为同一个块中没有重复声明

```javascript
var name = 'Nicholas';
console.log(name); // 'Nicholas'
if (true) {
  var name = 'Matt';
  console.log(name); // 'Matt'
}

let age = 30;
console.log(age); // 30
if (true) {
  let age = 26;
  console.log(age);  // 26
}
```

对声明冗余报错不会因混用let和var受影响

```javascript
var name;
let name; // SyntaxError

let age;
var age; // SyntaxError
```

#### 3.3.2.1 暂时性死区

let声明的变量不会在作用域中被提升

```javascript
console.log(name); // undefined
var name = 'Matt';

console.log(age); // ReferenceError: age没有定义
let age = 26;
```

在let声明之前的执行瞬间被称为“暂时性死区“，在此阶段引用任何后面才声明的变量都会抛出ReferenceError

#### 3.3.2.2 全局声明

使用let在全局作用域中声明的变量不会成为window对象的属性（var声明会）

```javascript
var name = 'Matt';
console.log(window.name); // 'Matt'

let age = 26;
console.log(window.age); // undefined
```

#### 3.3.2.3 条件声明

let声明的关键字不能依赖条件声明模式，因为它的作用域仅限于该块

#### 3.3.2.4 for循环中的let声明

for循环中定义的迭代变量会渗透到循环体外部

```javascript
for (var i = 0; i < 5; i++) {
  // 循环逻辑
}
console.log(i); // 5
```

改用let后问题就消失

使用var最常见的问题是对迭代变量的奇特声明和修改

```javascript
for (var i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 0)
}
// 实际会输出5、5、5、5、5
```

### 3.3.3 const声明

const与let基本相同，但是它声明变量时必须同时初始化变量，且修改const声明的变量会报错

```javascript
const age = 26;
age = 36; // TypeError: 给常量赋值

// const不允许重复声明
const name = 'Matt';
const name = 'Nicholas'; // SyntaxError

// const声明的作用域是块
const name = 'Matt';
if (true) {
  const name = 'Nicholas';
}
console.log(name); // Matt
```

const声明的限制只适用于它指向的变量的引用，即如果const变量引用的是一个对象，修改这个对象内部的属性并不违反const的限制

不可以用const声明迭代变量，但是可以声明一个不会修改的for循环变量

```javascript
for (const i = 0; i < 10; ++i) () // TypeError: 给常量赋值

let i = 0;
for (const j = 7; i < 5; ++i) {
  console.log(j);
}
// 7,7,7,7

for (const key in {a: 1, b: 2}) {
  console.log(key);
}
// a, b

for (const value of [1, 2, 3, 4, 5]) {
  console.log(value);
}
// 1, 2, 3, 4, 5
```

### 3.3.4 声明风格及最佳实践

* 不使用var
* const优先，let次之

## 3.4 数据类型

ECMAScript有6种简单数据类型（也称为原始类型）：Undefined, Null, Boolean, Number, String, Symbol, 1种复杂数据类型Object

### 3.4.1 typeof操作符

对一个值使用typeof操作符会返回下列字符串之一：

* "undefined" 未定义
* "boolean" 布尔值
* "string" 字符串
* "number" 数值
* "object" 对象（而不是函数）或null
* "function" 函数
* "symbol" 符号

> 严格来讲，函数在ECMAScript中被认为是对象，但是它又有自己特殊的属性，所以有必要通过typeof区分函数和其他对象

### 3.4.2 Undefined类型

此类型只有一个值，就是特殊值undefined，使用var或let声明了变量但未初始化时，就相当于赋予了该值

```javascript
let message;
console.log(message == undefined); // true
```

包含undefined值的变量和未定义变量是有区别的

```javascript
let message; 
// let age

console.log(message); // "undefined"
console.log(age); // 报错
```

然而对未声明的变量调用typeof依然会返回“undefined“

undefined是一个假值

```javascript
let message;

if (message) {
  // 这个块不会执行
}

if (!message) {
  // 这个块会执行
}

if (age) {
  // 这个块会报错
}
```

### 3.4.3 Null类型



