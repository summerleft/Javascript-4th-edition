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

#### 3.4.6.6 模板字面量标签函数

#### 3.4.6.7 原始字符串字面量

使用模板字面量可以直接获取原始的模板字面量内容（如换行符或Unicode字符）,可以使用默认的String的raw标签函数

```javascript
// Unicode示例
// \u00A9是版权符号
console.log(`\u00A9`);
console.log(String.raw`\u00A9`); // \u00A9

// 换行符示例
console.log(`first line\nsecond lind`);
// first line
// second line

console.log(String.raw`first line\nsecond line`); // "first line\nsecond line"

// 对实际的换行符来说是不行的
// 它们不会被转换为转义序列的形式
console.log(`first line
second line`);
// first line
// second line

console.log(String.raw`first line
second line`);
// first line
// second line
```

也可以通过标签函数的第一个参数，即字符串数组的raw属性取得每个字符串的原始内容

```javascript
function printRaw(strings) {
  console.log('Actual characters:');
  for (const string of strings) {
    console.log(string);
  }
  
  console.log('Escaped characters;');
  for (const rawString of strings.raw) {
    console.log(rawString);
  }
}

printRaw`\u00A9${ 'and' } \n`;
// Actual characters:
// ©️
// （换行符）
// Escaped characters:
// \u00A9
// \n
```

### 3.4.7 Symbol类型

ECMAScript6新增，符号是原始值，符号实例唯一、不可变。符号的用途是确保对象属性使用唯一标识符

#### 3.4.7.1 符号的基本用法

符号需要使用Symbol\(\)函数初始化。调用该函数时，可以传入字符串作为对符号的描述，可以通过这个字符串调试代码。但是这个字符串参数与符号定义或标识完全无关

```javascript
let genericSymbol = Symbol();
let otherGenericSymbol = Symbol();

let fooSymbol = Symbol('foo');
let otherFooSymbol = Symbol('foo');

console.log(genericSymbol == otherGenericSymbol); // false
consolt.log(fooSymbol == otherFooSymbol); // false
```

只要创建Symbol\(\)实例并将其用作对象的心属性，就可以保证它不会覆盖已有的对象属性，无论是符号属性还是字符串属性

```javascript
let genericSymbol = Symbol();
console.log(genericSymbol); // Symbol()

let fooSymbol = Symbol('foo');
console.log(fooSymbol); // Symbol(foo)
```

Symbol\(\)函数不能与new关键字一起作为构造函数使用，可以避免创建符号包装对象

```javascript
let mySymbol = new Symbol(); // TypeError: Symbol is not a constructor
```

如果确实需要使用符号包装对象，可以借用Object\(\)函数

```javascript
let mySymbol = Symbol();
let myWrappedSymbol = Object(mySymbol);
console.log(typeof myWrappedSymbol); // "object"
```

#### 3.4.7.2 使用全局符号注册表

如果运行时不同部分需要共享和重用符号实例，可以用一个字符串作为键，在全局符号注册表中创建并重用符号

```javascript
let fooGlobalSymbol = Symbol.for('foo');
console.log(typeof fooGlobalSymbol); // symbol
```

Symbol.for\(\)对每个字符串键都执行幂等操作。第一次使用某个字符串调用时，它会检查全局运行时的注册表，发现不存在对应的符号，于是就会生成一个新符号实例并添加到注册表中。后续使用相同字符串的调用同样会检查注册表，发现存在与该字符串对应的符号，然后就会返回该符号实例

```javascript
let fooGlobalSymbol = Symbol.for('foo'); // 创建新符号
let otherFooGlobalSymbol = Symbol.for('foo'); // 重用已有符号

console.log(fooGlobalSymbol === otherFooGlobalSymbol); // true
```

即使采用相同的符号描述，在全局注册表中定义的符号跟使用Symbol\(\)定义的符号也不相等

```javascript
let localSymbol = Symbol('foo');
let globalSymbol = Symbol.for('foo');

console.log(localSymbol === globalSymbol); // false
```

全局注册表中的符号必须使用字符串键来创建，因此作为参数传给Symbol.for\(\)的任何值都会被转换为字符串，且注册表中使用的键同时也会被用作符号描述

```javascript
let emptyGlobalSymbol = Symbol.for();
console.log(emptyGlobalSymbol); // Symbol(undefined)
```

可使用Symbol.keyFor\(\)查询全局注册表，该方法接收符号，返回该全局符号对应的字符串键，若查询的不是全局符号则返回undefined

```javascript
// 创建全局符号
let s = Symbol.for('foo');
console.log(Symbol.keyFor(s)); // foo

// 创建普通符号
let s2 = Symbol('bar');
console.log(Symbol.keyFor(s2)); // undefined
```

若传给Symbol.keyFor\(\)的不是符号，则抛出TypeError

#### 3.4.7.3 使用符号作为属性

可以使用字符串或数值作为属性的地方都可以使用符号，包括对象字面量属性和Object.defineProperty\(\)/Object.defineProperties\(\)定义的属性。对象字面量只能在计算属性语法中使用符号作为属性

### 3.4.8 Object类型

通过创建Object类型的实例创建自己的对象，再给对象添加属性和方法

```javascript
let o = new Object();
```

每个Object实例都有如下属性和方法

* constructor: 用于创建当前对象的函数
* hasOwnProperty\(propertyName\): 用于判断当前对象实例（不是原型）上是否存在给定的属性。要检查的属性名必须是字符串或符号
* isPrototypeOf\(object\): 用于判断当前对象是否为另一个对象的原型
* propertyIsEnumerable\(propertyName\): 用于判定给定的属性是否可以使用for-in语句枚举，属性名必须是字符串
* toLocaleString\(\): 返回对象的字符串表示，反映对象所在的本地化执行环境
* toString\(\): 返回对象的字符串表示
* valueOf\(\): 返回对象对应的字符串、数值或布尔值表示，通常与toString\(\)的返回值相同

## 3.5 操作符

包括数学操作符、位操作符、关系操作符和相等操作符等。在应用给对象时，操作符通常会调用valueOf\(\)和/或toString\(\)方法

### 3.5.1 一元操作符

#### 3.5.1.1 递增/递减操作符

前缀版和后缀版同C语言

递增和递减操作符遵循以下规则：

* 对于字符串，如果是有效的数值形式，则先转换为数值再应用改变
* 对于字符串，不是有效的数值形式，则设置为NaN
* 对于布尔值，false-&gt;0，true -&gt; 1
* 对于浮点值，正常执行
* 对于对象，则调用valueOf\(\)去的可操作的值，再应用上述规则。如果是NaN，则调用toString\(\)并再次应用规则

```javascript
let s1 = "2";
let s2 = "z";
let b = false;
let f = 1.1;
let o = {
  valueOf() {
    return -1'
  }
};

s1++ // 3
s2++; // NaN
b++; // 1
f-- ; // 0.10000000000000009
o--; // -2
```

#### 3.5.1.2 一元加和减

一元加（减）应用到非数值时，会执行与使用Number\(\)转型函数一样的类型转换

```javascript
let s1 = "01";
let s2 = "1.1";
let s3 = "z";
let b = false;
let f = 1.1;
let o = {
  valueOf() {
    return -1;
  }
};

s1 = +s1; // 1
s2 = +s2; // 1.1
s3 = +s3; // NaN
b = +b; // 0
f = +f; // 1.1
o = +o // -1
```

```javascript
let s1 = "01";
let s2 = "1.1";
let s3 = "z";
let b = false;
let f = 1.1;
let o = {
  valueOf() {
    return -1;
  }
};

s1 = -s1; // -1
s2 = -s2; // -1.1
s3 = -s3; // NaN
b = -b; // 0
f = -f; // -1.1
o = -o; // 1
```

### 3.5.2 位操作符

有符号整数使用32位的前31位表示整数值，第32位表示数值的符号

如果将位操作应用到非数值，首先会使用Number\(\)函数将值转换为数值，再应用操作

#### 3.5.2.1 按位非

```javascript
let num1 = 25;    // 二进制00000000000000000000000000011001
let num2 = ~num1; // 二进制11111111111111111111111111100110
console.log(num2);
```

#### 3.5.2.2 按位与

```javascript
let result = 25 & 3;
console.log(result); // 1
```

#### 3.5.2.3 按位或

```javascript
let result = 25 | 3;
console.log(result); // 27
```

#### 3.5.2.4 按位异或

```javascript
let result = 25 ^ 3;
console.log(result); // 26
```

#### 3.5.2.5 左移

```javascript
let oldValue = 2; // 二进制10
let newValue = oldValue << 5; // 二进制1000000
```

左移会保留它所操作数值的符号

#### 3.5.2.6 有符号右移

实际上是左移的逆运算

```javascript
let oldValue = 64;
let newValue = oldValue >> 5; 
```

右移后空位会出现在左侧，且在符号位之后，ECMAScript会用符号位的值来填充这些空位

#### 3.5.2.7 无符号右移

对正数来说，和有符号右移相同，但对负数来说，无符号右移会给空位补0

```javascript
let oldValue = -64;
let newValue = oldValue >>> 5; // 134217726
```

### 3.5.3 布尔操作符

一共有3个：逻辑非，逻辑与和逻辑或

#### 3.5.3.1 逻辑非

这个操作符始终返回布尔值，该操作符首先将操作数转换为布尔值，然后再取反，规则如下：

* 对象，返回false
* 空字符串，返回true
* 非空字符串，返回false
* 数值0，返回true
* 非0数值，返回false
* null，返回true
* NaN，undefined，返回true

同时使用两个叹号可以用于把任意值转换为布尔值

#### 3.5.3.2 逻辑与

逻辑与操作符可用于任何类型的操作数，如果有操作数不是布尔值，则逻辑与并不一定会返回布尔值，规则如下：

* 第一个操作数是对象，返回第二个操作数
* 第二个操作数是对象，只有第一个操作数求值为true才会返回该对象
* 两个操作数都是对象，返回第二个操作数
* 有一个操作数是null，返回null
* 有一个操作数是NaN，返回NaN
* 有一个操作数是undefined，返回undefined

逻辑与操作符是短路操作符，如果第一个操作数决定了结果，那么永远不会对第二个操作数求值

#### 3.5.3.3 逻辑或

规则如下

* 第一个操作数是对象，发挥第一个操作数
* 第一个操作数求值为false，返回第二个操作数
* 两个操作数都是对象，返回第一个操作数
* 两个操作数都是null/NaN/undefined，返回null/NaN/undefined

对逻辑或而言，第一个操作数求值为true，第二个操作数就不会再被求值

### 3.5.4 乘性操作符

3个乘性操作符：乘法、除法和取模。处理非数值时，会包含自动的类型转换。如果乘性操作符有不是数值的操作数，则会先使用Number\(\)转型函数转换为数值

#### 3.5.4.1 乘法操作符

特殊规则如下：

* 操作数都是数值，执行常规乘法运算，若不能表示乘积，返回Infinity或-Infinity
* 任一操作数是NaN，返回NaN
* Infinity \* 0 = NaN
* Infinity \* 非0有限数值，返回Infinity或-Infinity
* Infinity \* Infinity = Infinity
* 有不是数值的操作数，先在后台用Number\(\)转换为数值再应用上述规则

#### 3.5.4.2 除法操作符

特殊规则如下：

* 操作数都是数值，同乘法操作符
* 任意操作数是NaN，返回NaN
* Infinity / Infinity = NaN
* 0 / 0 = NaN
* 非0有限值 / 0 = Infinity或-Infinity
* Infinity / 任何数值，返回Infinity或-Infinity
* 有不是数值的操作数，先在后台用Number\(\)转换为数值再应用上述规则

### 3.5.5 指数操作符

```javascript
console.log(Math.pow(3, 2); // 9
console.log(3 ** 2); // 9

console.log(Math.pow(16, 0.5); // 4
console.log(16 ** 0.5); // 4

let squared = 3;
squard **= 2;
console.log(squared); // 9

let sqrt = 16;
sqrt **= 0.5;
console.log(sqrt); // 4
```

### 3.5.6 加性操作符

#### 3.5.6.1 加法操作符

规则如下：

* 任一操作数是NaN，返回NaN
* Infinity + Infinity = Infinity
* -Infinity + -Infinity = -Infinity
* Infinity + -Infinity = NaN
* +0 + +0 = +0
* -0 + =0 = +0
* -0 + -0 = -0

如果有一个操作数是字符串，应用如下规则：

* 两个操作数都是字符串，第二个字符串拼接到第一个字符串后面
* 只有一个操作数是字符串，将另一个操作数转换为字符串，再将两个字符串拼接





