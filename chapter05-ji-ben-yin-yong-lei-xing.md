# Chapter05 基本引用类型

引用类型有时候被称为对象定义，它们描述了自己的对象应有的属性和方法

> 引用类型有点像类，但跟类不是一个概念

对象被认为是某个特定引用类型的实例，新对象通过使用new操作符后跟一个构造函数来创建

## 5.1 Date

Date类型将日期保存为自协调世界时时间19790年1月1日午夜（零时）至今所经过的毫秒数

创建日期对象：

```javascript
let now = new Date();
```

在不给Date的构造函数传参数的情况下，创建的对象保存当前日期和时间。要基于其他日期和时间创建日期对象则须传入其毫秒表示

#### Date.parse\(\)

该方法接收一个表示日期的字符串参数，可将这个字符串转换为表示该日期的毫秒数，所有实现必须支持下列格式

* “5/23/2019“
* “May 23, 2019“
* “Tue May 23 2019 00:00:00 GMT-0800“
* 2019-05-23T00:00:00

```javascript
let someDate = new Date(Date.parse("May 23, 2019"));
let someDate = new Date("may 23, 2019");
```

若传给该方法的字符串不表示日期，返回NaN，直接把表示日期的字符串传给构造函数，则会在后台自动调用Date.parse\(\)

#### Date.UTC\(\)

同样返回日期的毫秒表示，传给该方法的参数是年、零起点月数（0-11）、日（1-31）、时（0-23）、分、秒和毫秒，只有年和月是必须的

```javascript
// GMT时间2000年1月1日零点
let y2k = new Date(Date.UTC(2000, 0));

// GMT时间2005年5月5日下午5点55分55秒
let allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));
```

Date.UTC\(\)也会被Date的构造函数隐式调用，但是这种情况下创建的事本地日期

```javascript
// 本地时间2000年1月1日零点
let y2k = new Date(2000, 0);

// 根底时间2005年5月5日下午5点55分55秒
let allFives = new Date(2005, 4, 5, 17, 55, 55);
```

#### Date.now\(\)

返回表示方法执行时日期和时间的毫秒数

```javascript
// 起始时间
let start = Date.now();

// 调用函数
doSomething();

// 结束时间
let stop = Date.now();
result = stop - start;
```

### 5.1.1 继承的方法

Date类型重写了toLocaleString\(\)、toString\(\)和valueOf\(\)方法

下面给出2019年2月1日零点的示例

```javascript
toLocaleString() - 2/1/2019 12:00:00 AM
toString() - Thu Feb 1 2019 00:00:00 GMT-0800 (Pacific Standard Time)
```

Date类型的valueOf\(\)方法不返回字符串，该方法被重写后返回的是日期的毫秒表示

```javascript
let date1 = new Date(2019, 0, 1); // 2019年1月1日
let date2 = new Date(2019, 1, 1); // 2019年2月1日

console.log(date1 < date2); // true
console.log(date1 > date2); // false
```

### 5.1.2 日期格式化方法

以下方法都会返回字符串：

* toDateString\(\)显示周几、月、日、年
* toTimeString\(\)显示日期中的时、分、秒和时区
* toLocaleDateString\(\)显示日期中的周几、月、日、年
* toLocaleTimeString\(\) 显示日期中的时、分、秒
* ToTUCString\(\)显示完整的UTC日期

这些方法的输出会因浏览器而异

## 5.2 RegExp

ECMAScript通过RegExp类型支持正则表达式

```javascript
let expression = /pattern/flags;
```

pattern是任意正则表达式，包括字符类、限定符、分组、向前查找和反响引用，每个正则表达式可以带零个或多个flags，表示比配模式的标记如下：

* g：全局模式，查找字符串全部内容，而不是找到第一个匹配的内容就结束
* i：不区分大小写
* m：多行模式
* y：粘附模式，表示只查找lastIndex开始之后的字符串
* u：Unicode模式
* s：dotAll模式，表示元字符匹配任何字符（包括\n或\r）

```javascript
// 匹配字符串中的所有"at"
let pattern1 = /at/g;

// 匹配第一个"bat"或"cat"，忽略大小写
let pattern2 = /[bc]at/i;

// 匹配所有以"at"结尾的三字组合、忽略大小写
let pattern3 = /.at/gi;
```

所有元字符在模式中必须转义

```javascript
// 匹配第一个"[bc]at"，忽略大小写
let pattern1 = /\[bc\]at/i;

// 匹配所有".at"，忽略大小写
let pattern2 = /\.at/gi;
```



