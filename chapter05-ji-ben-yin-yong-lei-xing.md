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

正则表达式也可以使用RegExp构造函数来创建，接收两个参数：模式字符串和（可选的）标记字符串

```javascript
let pattern1 = /[bc]at/i;

let pattern2 = new RegExp("[bc]at","i"];
```

### 5.2.1 RegExp实例属性

每个RegExp实例都有下列属性：

* global：布尔值，表示是否设置了g标记
* ignoreCase：g标记
* unicode：u标记
* sticky：y标记
* lastIndex：整数，表示在源字符串中下一次搜索的开始位置，始终从0开始
* multiline：m标记
* dotAll：s标记
* source：正则表达式的字面量字符串（不是传给构造函数的模式字符串），没有开头和结尾的斜杠
* flags：正则表达式的标记字符串，始终以字面量而非传入构造函数的字符串模式形式返回（没有前后斜杠）

```javascript
let pattern1 = /\[bc\]at\i;

console.log(pattern1.global); // false
console.log(pattern1.ignoreCase); //true
console.log(pattern1.multiline); // false
console.log(pattern1.lastIndex); // 0
console.log(pattern1.source);  // "\[bc/]at"
console.log(pattern1.flags); // "i"
```

无论是通过字面量创建还是通过RegExp构造函数创建，他们的属性都相同

### 5.2.2 RegExp实例方法

exec\(\)，用于配合捕获组使用，接收一个参数：要应用模式的字符串。若找到匹配项，返回包含第一个匹配信息的数组，没找到则返回null。返回的数组是Array的实例，包含两个额外的属性：index和input，index是字符串中匹配模式的起始位置，input是要查找的字符串。如果模式中没有捕获组，则数组只含有一个元素

```javascript
let text = "mom and dad and baby";
let pattern = /mom( and dad( and baby)?)?/gi;

let matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches.input); // "mom and dad and baby"
console.log(matches[0]); // "mom and dad and baby"
console.log(matches[1]); // " and dad and baby"
console.log(matches[2]); // " and baby"
```

如果设置了全局标记，每次调用exec\(\)方法会返回一个匹配的信息，如果没有设置，则无论对同一个字符串调用多少次exec\(\)，也只会返回第一个匹配的信息

```javascript
let text = "cat, bat, sat, fat";
let pattern = /.at/;

let matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat
console.log(pattern.lastIndex); // 0

matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat
console.log(pattern.lastIndex); // 0
```

lastIndex在非全局模式下始终不变。如果在模式上设置了g标记，则每次调用exec\(\)都会在字符串中向前搜索下一个匹配项

```javascript
let text = "cat, bat, sat, fat";
let pattern = /.at/g;
let matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat
console.log(pattern.lastIndex); // 3

matches = pattern.exec(text);
console.log(matches.index); // 5
console.log(matches[0]); // bat
console.log(pattern.lastIndex); // 8

matches = pattern.exec(text);
console.log(matches.index); // 10
console.log(matches[0]); // sat
console.log(pattern.lastIndex); // 13
```

全局匹配模式下，每次调用exec\(\)都会更新lastIndex值，以反映上次匹配的最后一个字符的索引

如果设置了黏附标记y，每次调用exec\(\)就只会在lastIndex的位置上寻找匹配项，且黏附标记覆盖全局标记

```javascript
let text = "cat, bat, sat, fat";
let pattern = /.at/y;

let matches = pattern.exec(text);
console.log(matches.index); // 0
console.log(matches[0]); // cat
console.log(pattern.lastIndex); // 3

matches = pattern.exec(text);
console.log(matches); // null;
console.log(patterns.lastIndex); // 0

pattern.lastIndex = 5;
matches = pattern.exec(text);
console.log(matches.index); // 5
console.log(matches[0]); // bat
console.log(pattern.lastIndex); // 8
```

另一个方法test\(\)，接收一个字符串参数参数。如果输入的文本与模式匹配，则返回true，否则返回false

```javascript
let text = "000-00-0000";
let pattern = /\d{3}-\d{2}-\d{4}/;

if (pattern.test(text)){
  console.log("The pattern was matched.");
}
```

无论正则表达式如何创建，toLocaleString\(\)和toString\(\)都返回正则表达式的字面量显示

```javascript
let pattern = new RegExp("\\[bc\\]at", "gi");
console.log(pattern.toString());  // /\[bc/]at/gi
console.log(pattern.toLocaleString()); // /\[bc/]at/gi
```

### 5.2.3 RegExp构造函数属性

| 全名 | 简写 | 说明 |
| :--- | :--- | :--- |
| input | $\_ | 最后搜索的字符串（非标准特性） |
| lastMatch | $& | 最后匹配的文本 |
| lastParen | $+ | 最后匹配的捕获组（非标准特性） |
| leftContext | $\` | input字符串中出现在lastMatch前面的文本 |
| rightContext  | $' | input字符串中出现在lastMatch后面的文本 |

## 5.3 原始包装类型

3种特殊引用类型：Boolean, Number, String

引用类型与原始值包装类型的区别在于对象的生命周期，通过new实例化引用类型得到的实例在离开作用域时被销毁，而自动创建的原始值包装对象只存在于访问它的那行代码执行期间

```javascript
let s1 = "some text";
s1.color = "red";
console.log(s1.color); // undefined
```

在原始值包装类型的实例上调用typeof会返回“object“，所有原始值包装对象会转换为布尔值true

Object构造函数能够根据传入值的类型返回相应原始值包装类型的实例

```javascript
let obj = new Object("some text");
console.log(obj instanceof String); // true
```

使用new调用原始值包装类型的构造函数与调用同名的转型函数不一样

```javascript
let value = "25";
let number = Number(value); // 转型函数
console.log(typeof number); // "number"
let obj = new Number(value); // 构造函数
console.log(typeof obj); // "object"
```

### 5.3.1 Boolean

Boolean实例会充血valueOf\(\)方法，返回原始值true或false，toString\(\)方法返回字符串“true“或“false“

> 强烈建议永远不要使用Boolean对象

### 5.3.2 Number

Number类型重写了valueOf\(\), toLocaleString\(\), toString\(\)，valueOf\(\)返回Number对象表示的原始数值，其他两个返回数字字符串，toString\(\)可选接收表示基数的参数

```javascript
let num = 10;
console.log(num.toString()); // "10"
console.log(num.toString(2)); // "1010"
console.log(num.toString(8)); // "12"
console.log(num.toString(10)); // "10"
console.log(num.toString(16)); // "a"
```

toFixed\(\)返回包含指定小数点位数的数值字符串

```javascript
let num = 10;
console.log(num.toFixed(2)); // "10.00"

let num = 10.005;
console.log(num.toFixed(2)); // "10.01"
```

toExponential\(\) 返回以科学记数法表示的数值字符串，接收参数表示结果中小数的位数

```javascript
let num = 10;
console.log(num.toExponential(1)); // "1.0e+1"
```

toPrecision\(\)返回最合理的输出结果，接收一个参数，表示结果中数字的总位数

```javascript
let num = 99;
console.log(num.toPrecision(1)); // "1e+2"
console.log(num.toPrecision(2)); // "99"
console.log(num.toPrecision(3)); // "99.0"
```

不建议直接实例化Number对象，在处理原始数值和引用数值时，typeof和instanceof会返回不同结果

```javascript
let numberObject = new Number(10);
let numberValue = 10;
console.log(typeof numberObject); // "object"
console.log(typeof numberValue);  // "number"
console.log(numberObject instanceof Number); // true
console.log(numberValue instanceof Number); // false
```

isIntjeger\(\)方法与安全整数，用于辨别一个数值是否为整数

```javascript
console.log(Number.isInteger(1)); // true
console.log(Number.isInteger(1.00)); // true
console.log(Number.isInteger(1.01)); // false
```

isSafeInteger\(\)方法

```javascript
console.log(Number.isSafeInteger(-1 * (2 ** 53))); // false
console.log(Number.isSafeInteger(-1 * (2 ** 53) + 1);; // true

console.log(Number.isSafeInteger(1 * (2 ** 53))); // false
console.log(Number.isSafeInteger((2 ** 53) - 1);; // true
```

### 5.3.3 String

String对象的方法可以在所有字符串原始值上调用，3个继承的方法valueOf\(\)，toLocaleString\(\)，toString\(\)都返回对象的原始字符串值

每个String对象都有属性length，表示字符串字符数量，双字节字符也按单字符计算

#### 5.3.3.1 JavaScript字符串

JavaScript字符串由16位码元组成，length属性表示字符串包含多少16位码元

```javascript
let message = "abcde";
console.log(message.length); // 5
```

charAt\(\)方法返回给定索引位置的字符

```javascript
let message = "abcde";
console.log(message.charAt(2)); // "c"
```

charCodeAt\(\)方法查看指定码元的字符编码

```javascript
let message = "abcde";
console.log(message.charCodeAt(2)); // 99
console.log(99 === 0x63); // true
```

fromCharCode\(\)方法根据给定的UTF-16码元创建字符串中的字符

```javascript
console.log(String.fromCharCode(0x61, 0x61, 0x63, 0x64, 0x65)); //"abcde"
console.log(String.fromCharCode(97, 98, 99, 100, 101)); //"abcde"
```

#### 5.3.3.2 normalize\(\)方法

#### 5.3.3.3 字符串操作方法

concat\(\) 拼接字符串

```javascript
let stringValue = "hello ";
let result = stringValue.concat("world");

console.log(result); // "hello world"
console.log(stringValue); // "hello"
```

```javascript
let stringValue = "hello ";
let result = stringValue.concat("world", "!");

console.log(result); // "hello world!"
```

加号操作符即可拼接字符串

提取子字符串的方法：slice\(\)，substr\(\)，substring\(\)。第一个参数表示子字符串开始的位置，第二个参数表示结束位置。slice\(\)和substring\(\)第二个参数是提取结束的位置，substr\(\)第二个参数表示返回的子字符串的字符数量，它们均不改变原来的字符串

```javascript
let stringValue = "hello world";
console.log(stringValue.slice(3)); // "lo world"
console.log(stringValue.substring(3)); // "lo world"
console.log(stringValue.substr(3)); // "lo world" 
console.log(stringValue.slice(3, 7)); // "lo w"
console.log(stringValue.substring(3, 7)); // "lo w"
console.log(stringValue.substr(3, 7)); //"lo worl"
```

参数为负数时，slice\(\)将负值参数当成字符串长度加上负参数值，substr\(\)将第一个负参数值当成字符串长度加上该值，第二个负参数值转换为0，substring\(\)将所有负参数值转换为0

```javascript
let stringValue = "hello world"; 
console.log(stringValue.slice(-3)); // "rld"
console.log(stringValue.substring(-3)); // "hello world"
console.log(stringValue.substr(03)); // "rld"
console.log(stringValue.slice(3, -4)); // "lo w"
console.log(stringValue.substring(3, -4)); // "hel"
console.log(stringValue.substr(3, -4)); // ""(empty)
```

#### 5.3.3.4 字符串位置方法

indexOf\(\)和lastIndexOf\(\)从字符串中搜索传入的字符串，并返回位置（未找到则返回-1），indexOf\(\)从字符串开头开始查找，lastIndexOf\(\)从字符串末尾开始查找

```javascript
let stringValue = "hello world";
console.log(stringValue.indexOf("o"); // 4
console.log(stringValue.lastIndexOf("o"); // 7
```

可选第二个参数，表示开始搜索的位置

```javascript
let stringValue = "hello world";
console.log(stringValue.indexOf("o", 6)); // 7
console.log(stringValue.lastIndexOf("o", 6)); // 4
```

#### 5.3.3.5 字符串包含方法

startsWith\(\)，endsWith\(\)，includes\(\)从字符串中搜索传入的字符串，返回一个表示是否包含的不二之，startsWith\(\)检查开始于索引0的匹配项，endsWith\(\)检查开始位于索引\(string.length-substring.length\)的匹配项，includes\(\)检查整个字符串

```javascript
let message = "foobarbaz";

console.log(message.startsWith("foo")); // true
console.log(message.startsWith("bar")); // false

console.log(message.endsWith("baz")); // true
console.log(message.endsWith("bar")); // false

console.log(message.includes("bar")); // true
console.log(message.includes("qux")); // false
```

startsWIth\(\)和includes\(\)方法接收可选第二参数，表示开始搜索位置

```javascript
let message = "foobarbaz";

console.log(message.startsWith("foo")); // true
console.log(message.startsWith("foo", 1)); // false

console.log(message.includes("bar")); // true
console.log(message.includes("bar", 4)); // false

```

endsWith\(\)接收可选第二参数，表示当作字符串末尾的位置

```javascript
let message = "foobarbaz";

console.log(message.endsWith("bar")); // false
console.log(message.endsWith("bar", 6)); // true
```

#### 5.3.3.6 trim\(\)方法

创建字符串的一个副本，删除前后所有空格

```javascript
let stringValue = "  hello world ";
let trimmedStringValue = stringValue.trim();
console.log(stringValue); // "  hello world "
console.log(trimmedStringValue); // "hello world"
```

#### 5.3.3.7 repeat\(\)方法

接收一个整数参数，表示将字符串复制多少次，返回拼接所有副本后的结果

```javascript
let stringValue = "na ";
console.log(stringValue.repeat(16) + "batman");
// na na na na na na na na na na na na na na na na batman
```

#### 5.3.3.8 padStart\(\)和padEnd\(\)

复制字符串，若小于指定长度，则在相应一边填充字符，直到满足条件，第一个参数是长度，第二个参数时可选的填充字符串，默认为空格

```javascript
let stringValue = "foo";

console.log(stringValue.padStart(6)); // "   foo"
console.log(stringValue.padStart(9, ".")); // "......foo"

console.log(stringValue.padEnd(6)); // "foo . "
console.log(stringValue.padEnd(9, ".")); // "foo......"
```

第二个参数可以是字符串

```javascript
let stringVlue = "foo";

console.log(stringValue.padStart(8, "bar")); // "barbafoo"
console.log(stringValue.padStart(2)); // "foo"

console.log(stringValue.padEnd(8, "bar")); // "foobarba"
console.log(stringValue.padEnd(2)); // "foo"
```

#### 5.3.3.9 字符串迭代与解构

```javascript
let message = "abc";
let stringIterator = message[Symbol.iterator]();

conlole.log(stringIterator.next()); // {value: "a", done: false}
conlole.log(stringIterator.next()); // {value: "b", done: false}
conlole.log(stringIterator.next()); // {value: "c", done: false}
conlole.log(stringIterator.next()); // {value: "undefined", done: true}

for (const c of "abcde") {
  console.log(c);
}

// a
// b
// c
// d
// e
```

字符串可以通过解构操作解构

```javascript
let message = "abcde";
console.log([...message]); // ["a", "b", "c", "d", "e"]
```

#### 5.3.3.10 字符串大小写转换

* toLowerCase\(\)
* toLocaleLowerCase\(\)
* toUpperCase\(\)
* toLocaleUpperCase\(\)

两个带Locale的方法用于某些特殊地区语言

#### 5.3.3.11 字符串模式匹配方法

match\(\)本质上和RegExp的exec\(\)相同，接收一个参数，可以是正则表达式字符串或RegExp对象

```javascript
let text = "cat, bat, sat, fat";
let pattern = /.at/;

// 等价于pattern.exec(text)
let matches = text.match(pattern);
console.log(matches.index); // 0
console.log(matches[0]); // "cat"
console.log(pattern.lastIndex); // 0
```

search\(\)，唯一参数与match\(\)一样，方法返回模式第一个匹配的位置索引，没找到则返回-1，从头至尾匹配

```javascript
let text = "cat, bat, sat, fat";
let pos = text.search(/at/);
console.log(pos); // 1
```

replace\(\)，接收两个参数，第一个是RegExp对象或字符串，第二个参数是字符串或者函数。若第一个参数是字符串，则只替换第一个子字符串，若替换所有用子字符串，则第一个参数必须是正则表达式且带全局标记

```javascript
let text = "cat, bat, sat, fat";
let result = text.replace("at", "ond");
console.log(result); // "cond, bat, sat, fat"

result = text.replace(/at/g, "ond");
console.log(result); // "cond, bond, sond, fond"
```

#### 5.3.3.12 localeCompare\(\)

比较两个字符串，返回值如下：

* 按照字母表顺序，字符串排在字符串参数前面，则返回负值（通常为-1）
* 字符串与字符串参数相等，返回0
* 按照字母表顺序，字符串排在字符串参数后面，则返回正值

```javascript
let stringValue = "yellow";
console.log(stringValue.localeCompare("brick")); // 1
console.log(stringValue.localeCompare("yellow")); // 0
console.log(stringValue.localeCompare("zoo")); // -1
```

