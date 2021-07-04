# Chapter06 集合引用类型

## 6.1 Object

Object实例适合存储和在应用程序间交换数据

显式创建Object实例有两种方式，第一种是使用new操作符和Object构造函数

```javascript
let person = new Object();
person.name = "Nocholas";
person.age = 29;
```

也可使用对象字面量表示法

```javascript
let person = {
  name: "Nicholas",
  age: 29
};
```

字面量表示法中，属性名可以是字符串或数值\(数值属性会自动转换为字符串\)

```javascript
let person = {
  "name": "Nicholas",
  "age": 29,
  5: true
};
```

获取属性一般通过点语法，也可使用中括号存取属性，使用中括号时，括号内使用属性名的字符串形式

```javascript
console.log(person["name"]); // "Nicholas"
console.log(person.name); // "Nicholas"
```

如果属性名中包含可能会导致语法错误的字符，或者包含关键字/保留字时，可使用中括号语法

```javascript
person["first name"] = "Nicholas";
```

## 6.2 Array

数组中每个槽位可以存储任意类型的数据，数组是动态大小

### 6.2.1 创建数组

使用Array构造函数

```javascript
let color = new Array();
```

给构造函数传入一个数值，则lenth属性会被创建且设置为这个值

```javascript
let colors = new Array(20);
```

可以给Array构造函数传入要保存的元素

```javascript
let colors = new Array("red", "blue", "green");
```

若创建数组时给构建函数的值是数值以外的类型，则创建只包含该值的数组

```javascript
let colors = new Array(3); // 包含3个元素的数组
let names = new Array("Greg"); // 只包含字符串"Greg"的数组
```

使用Array构造函数可省略new

```javascript
let colors = Array(3);
let names = Array("Greg");
```

使用数组字面量创建数组

```javascript
let colors = ["red", "blue", "green"];
let names = [];
let values = [1,2,];
```

> 使用数组字面量表示法创建数组时不调用Array构造函数

ES6新增：from\(\)和of\(\)，from\(\)将类数组结构转换为数组实例，of\(\)将一组参数转换为数组实例

Array.from\(\)的第一个参数是类数组对象，即任何可迭代结构，或有一个length属性和可索引元素的结构

```javascript

```

