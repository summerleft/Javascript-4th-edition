# Chapter04 变量、作用域与内存

## 4.1 原始值与引用值

原始值就是最简单的数据，引用值则是由多个值构成的对象

6种原始值：Undefined、Null、Boolean、Number、String、Symbol，保存原始值的变量按值访问

引用值是保存在内存中的对象，js不允许直接访问内存位置，我们操作的实际是对该对象的引用，所以保存引用值的变量是按引用访问

> 很多语言字符串是使用对象表示的，即引用类型，但是ECMAScript打破了这个惯例

### 4.1.1 动态属性

引用值可以随时添加、修改和删除其属性和方法

```javascript
let person = new Object();
person.name = "Nicholas";
console.log(person.name); // "Nicholas"
```

原始值不能有属性，尽管尝试添加属性不会报错

```javascript
let name = "Nicholas";
name.age = 27;
console.log(name.age); // undefined
```

原始类型的初始化可以只使用原始字面量形式。如果使用new关键字，JavaScript会创建一个Object类型的实例，但其行为类似原始值

```javascript
let name1 = "Nicholas";
let name2 = new String("Matt");
name1.age = 27;
name2.age = 26;
console.log(name1.age); // undefined
cnosole.log(name2.age); // 26
console.log(typeof name1); // string
console.log(typeof name2); // object
```

### 4.1.2 复制值

通过变量把一个原始值赋值到另一个变量时，原始值会被复制到新变量的位置

而在把引用值从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在的位置，但是这里复制的值实际上是一个指针，所以两个变量实际上指向同一个对象，因此一个对象上面的变化会在另一个对象上反映出来

```javascript
let obj1 = new Object();
let obj2 = obj1;
obj1.name = "Nicholas";
console.log(obj2.name); // "Nicholas"
```

### 4.1.3 传递参数

ECMAScript中所有函数的参数都是按值传递的。如果是原始值，就跟原始值变量的复制一样，如果是引用值，就跟引用值的复制一样

```javascript
function addTen(num) {
  num += 10;
  return num;
}

let count = 20;
let result = addTen(count);
console.log(count); // 20
console.log(result); // 30
```

```javascript
function setName(obj) {
  obj.name = "Nicholas";
}

let person = new Object();
setName(person);
console.log(person.name); // "Nicholas"
```

```javascript
function setName(obj) {
  obj.name = "Nicholas";
  obj = new Object();
  obj.name = "Greg";
}

let person = new Object();
setName(person);
console.log(person.name); // "Nicholas"
```

> ECMAScript中函数的参数就是局部变量

### 4.1.4 确定类型

ECMAScript提供了instanceof操作符，可以知道一个值是什么类型的对象，语法如下

```javascript
result = varable instanceof constructor
```

如果变量是给定引用类型的实例，用instanceof操作符返回true

```javascript
console.log(person instanceof Object); 
console.log(colors instanceof Array);
console.log(pattern instanceof RegExp);
```

用instanceof检测原始值，始终返回false，因为原始值不是对象

## 4.2 执行上下文与作用域

变量或函数的上下文决定了它们可以访问哪些数据，以及它们的行为。每个上下文都有一个关联的变量对象，这个上下文中定义的所有变量和函数都在于这个对象上。

全局上下文是最外层的上下文。在浏览器中，全局上下文是window对象，通过var定义的全局变量和函数会成为window对象的属性和方法，使用let和const的顶级声明不会定义在全局上下文中

代码执行时，标识符解析通过沿作用域链逐级搜索标识符名称完成，搜索过程始终从作用域链的最前端开始，逐级往后

```javascript
var color = "blue";

function changeColor() {
  if (color === "blue") {
    color = "red";
  } else {
    color = "blue";
  }
}

changeColor();
```

函数changeColor\(\)的作用域链包含两个对象：自己的变量对象和全局上下文的变量对象，函数内部之所以能够访问变量color就是因为可以在作用域链中找到它

局部作用域中定义的变量可用于在局部上下文中替换全局变量

```javascript
var color = "blue";

function changeColor() {
  let anotherColor = "red";
  
  function swapColors() {
    let tempColor = anotherColor;
    anotherColor = color;
    color = tempColor;
    
    // 这里可以访问color、anotherColor和tempColor
  }
    
  // 这里可以访问color和anotherColor，访问不到tempColor
  swapColors();
}

// 这里只能访问color
changeColor();  
```

内部上下文可以通过作用域链访问外部上下文中的一切，但外部上下文无法访问内部上下文中的任何东西

> 函数参数被认为是当前上下文中的变量，因此也跟上下文中的其他变量遵循相同的访问规则

### 4.2.1 作用域链增强

某些语句会在作用域链前端临时添加一个上下文，在代码执行后会被删除，通常会在以下两种情况下出现：

* try/catch语句的catch块
* with语句

对with语句来说，会向作用域链前端添加指定的对象；catch语句则会创建一个新的变量对象，它会包含要抛出的错误对象的声明

```javascript
function buildUrl() {
  let qs = "?debug=true";
  
  with(location) {
    let url = href + qs;
  }
  
  return url;
}
```

with语句将location对象作为上下文，location会被添加到作用域链前端。with语句中的代码引用变量href时，实际上引用的事location.href，引用qs时，引用的则是定义在buildUrl\(\)中的那个变量，它定义在函数上下文的变量对象上，with语句中使用var声明的变量url会成为函数上下文的一部分，但let声明的url因为被限制在会级作用域，所以在with块之外没有定义

### 4.2.2 变量声明

#### 4.2.2.1 使用var的函数作用域声明

var声明的变量会被自动添加到最接近的上下文，函数中为函数的局部上下文，with语句中为函数上下文，若变量未经声明就被初始化就会被添加到全局上下文

```javascript
function add(num1, num2) {
  var sum = num1 + num2;
  return sum;
}

let result = add(10, 20); // 30
console.log(sum); // 报错：sum在这里不是有效变量
```

如果省略上面例子中的var，sum在add\(\)被调用后就可以访问

```javascript
function add(num1, num2) {
  sum = num1 + num2;
  return sum;
}

let result = add(10, 20); // 30
console.log(sum); // 30
```

> 未经声明而初始化变量是JavaScript编程中一个非常常见的错误，会导致很多问题。为此，读者在初始化变量之前一定要先声明变量。在严格模式下，未经声明就初始化变量会报错

var声明会被拿到函数或全局作用域的顶部，位于作用域中所有代码之前，这种现象叫做“提升“。通过在声明之前打印变量，可以验证变量会被提升

```javascript
console.log(name); // undefined
var name = 'Jake';

functiuon() {
  console.log(name); // undefined
  var name = 'Jake';
}
```

#### 4.2.2.2 使用let的块级作用域声明

let的作用域是块级的，块级作用域由最近的一对包含花括号界定

let在同一作用域内不能声明两次，而重复的var声明会被忽略

```javascript
var a;
var a;
// 不会报错

{
  let b;
  let b;
}
// SyntaxError: 标识符b已经声明过了
```

let非常适合在循环中声明迭代变量，使用var声明的迭代变量会泄漏到循环外部

#### 4.2.2.3 使用const的常量声明

使用const声明的变量必须同时初始化为某个值，一经声明，在其声明周期的任何时候都不能再重新赋予新值

```javascript
const a; // SyntaxError: 常量声明时没有初始化

const b = 3;
console.log(b); // 3
b = 4; // TypeError: 给常量赋值
```

其他方面与let声明是一样的

const声明只应用到顶级原语或者对象，即赋值为对象的const变量不能再被重新赋值为其他引用值，但对象的键不受限制

```javascript
const o1 = {};
o1 = {}; // TypeError: 给常量赋值

const o2 = {};
o2.name = 'Jake';
console.log(o2.name); // 'Jake'
```

使用Object.freeze\(\)可以让整个对象都不能修改，虽然这样给属性赋值时不会报错，但是静默失败

```javascript
const o3 = Object.freeze({});
o3.name = 'Jake';
console.log(o3.name); // undefined
```

> 如果开发流程不会因此受很大影响，应该尽可能多使用const声明

#### 4.2.2.4 标识符查找

```javascript
var color = 'blue';

function getColor() {
  return color;
}

console.log(getColor()); // 'blue'
```

函数调用时会引用变量color，此时会进行两部搜索。第一步，搜索getColor\(\)的变量对象，查找名为color的标识符。结果没找到，于是继续搜索下一个变量对象（来自全局上下文），就找到了名为color的标识符。

对这个搜索过程而言，引用局部变量会让搜索自动停止，也就是说，如果局部上下文中有一个同名的标识符，那就不能在该上下文中的同名标识符

```javascript
var color = 'blue';

function getColor() {
  let color = 'red';
  return color;
}

console.log(getColor()); // 'red'
```

## 4.3 垃圾回收

JavaScript通过自动内存管理实现内存分配和闲置资源回收。基本思路：确定哪个变量不会再使用，就释放它占用的内存。这个过程是周期性的，即垃圾回收每隔一定时间就会自动运行

浏览器的发展史上用过两种主要的标记策略：标记清理和引用计数

### 4.3.1 标记清理

垃圾回收程序运行的时候，会标记内存中存储的所有变量，然后会将所有再上下文中的变量，以及被在上下文中的变量的标记去掉，在此之后再被加上标记的变量就是待删除的了，因为任何在上下文中的变量都访问不到它们了

### 4.3.2 引用计数

该策略不常用。其思路是对每个值都记录它被引用的次数，声明变量并给它赋一个引用值时，这个值的引用数为1，如果同一个值又被赋给另一个变量，那么引用数加1，如果保存对该值引用的变量被其他值给覆盖了，那么引用数减1，引用数为0时，就说明没办法再访问到这个值了

### 4.3.3 性能

### 4.3.4 内存管理

将内存占用量保持在一个较小的值可以让页面性能更好。优化内存占用的最佳手段就是保证在执行代码时只保存必要的数据。如果数据不再必要，就把它设置为null，这也叫解除引用。

```javascript
function createPerson(name) {
  let localPerson = new Object();
  localPerson.name = name;
  return localPerson;
}

let globalPerson = createPerson("Nicholas");

// 解除globalPerson对值的引用

globalPerson = null;
```

#### 4.3.4.1 通过const和let声明提升性能

相比于使用var，使用这两个新关键字可能会更早地让垃圾回收程序介入

#### 4.3.4.2 隐藏类和删除操作

#### 4.3.4.3 内存泄露

#### 4.3.4.4 静态分配与对象池

