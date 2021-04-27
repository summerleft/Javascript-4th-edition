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

