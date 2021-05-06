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

Null类型只有一个值null，null值表示一个空指针

```javascript
let car = null;
console.log(typeof car); // "object"
```

在定义将来要保存对象值的变量时，建议使用null来初始化

undefined值由null值派生而来，因此ECMA-262将他们定义为表面相等

```javascript
console.log(null == undefined); // true
```

### 3.4.4  Boolean类型

有两个字面值：true和false

不同类型与布尔值之间的转换规则：

| 数据类型 | 转换为true的值 | 转换为false的值 |
| :--- | :--- | :--- |
| Boolean | true | false |
| String | 非空字符串 | 空字符串 |
| Number | 非零数值（包括无穷值） | 0、NaN |
| Object | 任意对象 | null |
| Undefined | 不存在 | undefined |

if等流控制语句会自动执行其他类型值到布尔值的转换

### 3.4.5 Number类型

```javascript
let intNum = 55; // 十进制整数
let octalNum1 = 070; // 八进制56
let hexNum = 0xA; // 十六进制10
```

十六进制中的字母大小写均可

#### 3.4.5.1 浮点值

数值中必须包含小数点，小数点后必须至少有一个数字，小数点前面不必须有整数但推荐加上

在小数点后面没有数字的情况下，数值会自动变成整数，即使跟着0，也会被转换为整数

科学计数法

```javascript
let floatNum = 3.125e7;
```

浮点值精确度最高可达17位小数，但不如整数精确，0.1+0.2会得到0.300 000 000 000 000 04，因此永远不要测试某个特定的浮点值

#### 3.4.5.2 值的范围

ECMAScript可以表示的最小值和最大值分别保存在Number.MIN\_VALUE中和Number.MAX\_VALUE中

若计算得到的结果超出了可以表示的范围，数值会被自动转换为Infinity或-Infinity，且不可用于任何计算

确定一个值是不是有限大，可使用isFinite\(\)函数

```javascript
let result = Number.MAX_VALUE + Number.MAX_VALUE;
console.log(isFinite(result)); // false;
```

> Number.NEGATIVE\_INFINITY和Number.POSITIVE\_INFINITY可以获取正、负Infinity

#### 3.4.5.3 NaN

用于表示本来要返回数值的操作失败了（而不是抛出错误）

```javascript
console.log(0/0); // NaN
console.log(-0/+0); // NaN
console.log(5/0); // Infinity
console.log(5/-0); // -Infinity任何
```

任何涉及NaN的操作始终返回NaN，NaN不等于包括NaN在内的任何值

isNaN\(\)函数

```javascript
console.log(isNaN(NaN)); // true
console.log(isNaN(10); // false
console.log(isNaN("10")); // false，可以转换为数值10
console.log(isNaN("blue")); // true，不可以转换为数值
console.log(isNaN(true); // false，可以转换为数值1
```

> isNaN\(\)可以用于测试对象。它首先会调用对象的valueOf\(\)方法，然后确定返回值是否可以转化为数值，如果不能，再调用toString\(\)方法

#### 3.4.5.4 数值转换

Number\(\)、parseInt\(\)、parseFloat\(\)可将非数值转换为数值，Number\(\)可用于任何数据类型，后两个用于将字符串转换为数值

Number\(\)转换规则

* true-&gt;1，false-&gt;0
* 数值，直接返回
* null-&gt;0
* undefined-&gt;NaN
* 字符串
  * 包含数值字符，转换为十进制数值
  * 包含有效的浮点值格式，转换为相应浮点值
  * 包含有效的十六进制格式"0xf"，转换为对应的十进制整数值
  * 空字符串，返回0
  * 包含除上述情况之外的其他字符，返回NaN
* 对象，调用valueOf\(\)，若结果是NaN，调用toString\(\)再转换

```javascript
let num1 = Number("Hello world!"); // NaN
let num2 = Number(""); // 0
let num3 = Number("000011"); // 11
let num4 = Number(true); // 1
```

parseInt\(\)会忽略字符串最前面的空格，若第一个字符不是数值字符或加减号，立即返回NaN，空字符也会返回NaN。如果字符串以"0x"开头，会被解释为十六进制整数，以”0“开头且紧跟数值字符，在非严格模式下会被某些实现解释为八进制整数

```javascript
let num1 = parseInt("1234blue"); // 1234
let num2 = parseInt(""); // NaN
let num3 = parseInt("0xA"); // 10
let num4 = parseInt("22.5"); // 22
let num5 = parseInt("70"); // 70
let num6 = parseInt("0xf"); // 15
```

parseInt\(\)接收第二个参数，用于指定进制数

```javascript
let num = parseInt("0xAF", 16); // 175
let num1 = parseInt("AF", 16); // 175
let num2 = parseInt("AF"); // NaN
```

parseFloat\(\)函数类似，第一次出现的小数点有效，但第二次出现的小数点就无效，剩余字符会被忽略，且始终忽略字符串开头的0，不能指定进制数。若字符串表示整数（没有小数点或小数点后只有一个0），则返回整数

```javascript
let num1 = parseFloat("1234blue"); // 1234
let num2 = parseFloat("0xA"); // 0
let num3 = parseFloat("22.5"); // 22.5
let num4 = parseFloat("22.34.5"); // 22.34
let num5 = parseFloat("0908.5"); // 908.5
let num6 = parseFloat("3.125e7"); // 3125000
```

### 3.4.6 String类型

字符串可以使用双引号\("\)、单引号\('\)或反引号\(\`\)表示，但是不可混用

#### 3.4.6.1 字符字面量

| 字面量 | 含义 |
| :--- | :--- |
| \n | 换行 |
| \t | 制表 |
| \b | 退格 |
| \r | 回车 |
| \f | 换页 |
| \\ | 反斜杠 |
| \' | 单引号 |
| \" | 双引号 |
| \\` | 反引号 |
| \xnn | 以十六进制编码nn表示的字符，如\x41等于"A" |
| \unnnn | 以十六进制编码nnnn表示的Unicode字符 |

转义序列表示一个字符

> 若字符串中包含双字节字符，那么length属性返回的值可能不是准确的字符数

#### 3.4.6.2 字符串的特点

ECMAScript中字符串不可变，即一旦创建，值就不能变。要修改某个变量中的字符串，必须先销毁原始字符串

#### 3.4.6.3 转换为字符串

toString\(\)方法可见于数值、布尔值、对象和字符串，null和undefined没有toString\(\)方法

大多情况下toString\(\)不接受参数，但对数值调用时，可接收底数参数

```javascript
let num = 10;
console.log(num.toString()); // "10"
console.log(num.toString(2)); // "1010"
console.log(num.toString(8)); // "12"
console.log(num.toString(10)); // "10"
console.log(num.toString(16)); // "a"
```

> 用加号操作符给一个值加上空字符串也可以将其转换为字符串

#### 3.4.6.4 模板字面量

反引号内部的空格会被保留，在使用时要格外注意，格式正确的模板字符串看起来可能会缩进不当

```javascript
// 这个模板字面量在换行符之后有25个空格符
let myTemplateLiteral = `first line
                         second line`;
console.log(myTemplateLiteral.length); // 47

// 这个模板字面量以一个换行符开头
let secontTemplateLiteral = `
first line
second line`;
console.log(secondTemplateLiteral[0] === '\n'); // true

//这个模板字面量没有意料之外的字符
let thirdTemplateLiteral = `first line
second lone`;
console.log(thirdTemplateLiteral);
// first line
// second line
```

#### 3.4.6.5 字符串插值

字符串插值通过在${ }中使用一个JavaScript表达式实现

```javascript
let value = 5;
let exponent = 'second';
// 以前，字符串插值是这样实现的
let interpolatedString = 
  value + ' to the ' + exponent + ' power is ' + (value * value);
  
// 现在，可以用模板字面量这样实现
let interpolatedTemplateLiteral = 
  `${ value } to the ${ exponent } power is ${ value * value }`;
  
console.log(interpolatedString); // 5 to the second power is 25
console.log(interpolatedTemplateLiteral); // 5 to the second power is 25
```

所有插入的值都会使用toString\(\)强制转型为字符串，任何JavaScript表达式都可以用于插值，嵌套的模板字符串无须转义

