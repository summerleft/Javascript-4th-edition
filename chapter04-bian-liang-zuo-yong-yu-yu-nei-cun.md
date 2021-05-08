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



