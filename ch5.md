## 第五章 基本引用类型

引用类型是把数据和功能组织到一起的结构，引用类型虽然有点像类，但跟类并不是一个概念。引用类型也被称为**对象定义**，因为它们描述了自己的对象应有的属性和方法

对象被认为是某个特定引用类型的**实例**。新对象通过使用 new 操作符后跟一个构造函数（constructor）
来创建。构造函数就是用来创建新对象的函数

函数也是一种引用类型。

### 5.1 Date

Data 类型为自协调世界时（UTC，Universal  Time  Coordinated）时间 1970 年 1 月 1 日午夜（零时）至今所
经过的毫秒数。

`Date.parse(`) : 接收一个表示日期的字符串参数，尝试将这个字符串转换为表示该日期的毫秒数。

字符串为以下格式

* “月/日/年”，如 "5/23/2019" ； 
*  “月名 日, 年”，如 "May 23, 2019" ； 
* “周几 月名 日 年 时:分:秒 时区”，如 "Tue May 23 2019 00:00:00 GMT-0700" ； 
* ISO  8601 扩展格式“YYYY-MM-DDTHH:mm:ss.sssZ”，如 2019-05-23T00:00:00 （只适用于兼容 ES5 的实现）。 

如果传给 Date.parse() 的字符串并不表示日期，则该方法会返回 NaN 。

如果直接把字符串传给 Date 构造函数，那么 Date 会在后台调用 Date.parse() 。

`Date.UTC()` 功能与`Date.parse`一样，但是传入参数是年(必须)、零起点月数(必须)（1 月是 0，2 月是 1，以此类推）、日（1~31）、时（0~23）、分、秒和毫秒。例如

```javascript
// GMT 时间 2000 年 1 月 1 日零点 
let y2k = new Date(Date.UTC(2000, 0)); 
 
// GMT 时间 2005 年 5 月 5 日下午 5 点 55 分 55 秒 
let allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55)); 
```



Date.now() 返回表示方法执行时日期和时间的毫秒数。这个方法可以方便地用在代码分析中： 

```javascript
// 起始时间 
let start = Date.now(); 
 
// 调用函数 
doSomething(); 
 
// 结束时间 
let stop = Date.now(), 
result = stop - start; 
```

Date 类型重写了 `toLocaleString()` 、` toString() `和 `valueOf()` 方法

`toLocalString()`方法返回与浏览器运行的本地环境一致的日期和时间

`toString()` 方法返回带时区信息的日期和时间，而时间也是以 24 小时制（0~23）表示的。

```javascript
toLocaleString() - 2/1/2019 12:00:00 AM 
 
toString() - Thu Feb 1 2019 00:00:00 GMT-0800 (Pacific Standard Time) 
```

`valueOf()` 方法返回日期的毫秒表示。

**日期格式化方法**

 `toDateString() `显示日期中的周几、月、日、年（格式特定于实现）； 
 `toTimeString()` 显示日期中的时、分、秒和时区（格式特定于实现）； 
 `toLocaleDateString()` 显示日期中的周几、月、日、年（格式特定于实现和地区）； 
 `toLocaleTimeString()` 显示日期中的时、分、秒（格式特定于实现和地区）； 
 `toUTCString()` 显示完整的 UTC 日期（格式特定于实现）。 

#### 5.2 RegExp

正则表达式创建：

```javascript
let expression = /pattern/flags;
```

 每个正则表达式可以带零个或多个 flags （标记）。表示匹配模式的标记：

* g ：全局模式，表示查找字符串的全部内容，而不是找到第一个匹配的内容就结束。 
*  i ：不区分大小写，表示在查找匹配时忽略 pattern 和字符串的大小写。 
* m ：多行模式，表示查找到一行文本末尾时会继续查找。 
* y ：粘附模式，表示只查找从 lastIndex 开始及之后的字符串。 
* u ：Unicode 模式，启用 Unicode 匹配。 
* s ： dotAll 模式，表示元字符 . 匹配任何字符（包括 \n 或 \r ）。 

实例方法

主要是`exec()`。该方法只接收一个参数，即要应用模式的字符串。

如果没找到匹配项，则返回null 。

如果找到了匹配项，则返回包含第一个匹配信息的数组。这个array两个额外的属性：

*  index 是字符串中匹配模式的起始位置
*  input 是要查找的字符串。

数组的第一个元素是匹配整个模式的字符串，其他元素是与表达式中的捕获组匹配的字符串。例如

```javascript
let text = "mom and dad and baby"; 
let pattern = /mom( and dad( and baby)?)?/gi; 
 
let matches = pattern.exec(text); 
console.log(matches.index);   // 0 
console.log(matches.input);   // "mom and dad and baby" 
console.log(matches[0]);      // "mom and dad and baby" 
console.log(matches[1]);      // " and dad and baby" 
console.log(matches[2]);      // " and baby" 
```

### 5.3 原始值包装类型

ECMAScript 提供了  3 种特殊的引用类型： Boolean 、 Number 和 String ，它们与各自对应原始类型有的特殊行为。每当用到某个原始值的方法或属性时，后台都会创建一个相应**原始包装类型**的对象，使得原始值拥有对象的行为。

#### 1. Boolean

Boolean 是对应布尔值的引用类型。创建一个 Boolean 对象，使用 Boolean 构造函数并传入true 或 false ，如下例所示：

```javascript
let booleanObject = new Boolean(true); 
```

Boolean 的`valueOf()`方法会返回一个原始值 true 或 false 。 

`toString()` 方法返回字符串 "true" 或 "false" 。

#### 2. Number

Number 是对应数值的引用类型。创建时使用Number构造函数传入一个数值。

```javascript
let numberObject = new Number(10); 
```

Number 类型重写了 `valueOf()` 、 `toLocaleString()` 和 `toString()` 方法。`valueOf()` 方法返回 Number 对象表示的原始数值，另外两个方法返回数值字符串。

` toString()`方法可选地接收一个表示基数的参数，并返回相应基数形式的数值字符串。

格式化数值字符串可以使用下列方法

`toFixed()` 方法返回包含指定小数点位数的数值字符串。接收的参数表示小数的位数。

`toExponential()` ，返回以科学记数法（也称为指数记数法）表示的数值字符串。接收的参数表示小数的位数。

`toPrecision()` 方法会根据情况返回最合理的输出结果。它会根据数值和精度来决定调用 `toFixed()` 还是 `toExponential() `。

#### 3. String

String 是对应字符串的引用类型。创建时使用 String 构造函数并传入一个数值。

```javascript
let stringObject = new String("hello world"); 
```

三个方法 `valueOf()` 、 `toLocaleString()` 和 `toString()`都返回对象的原始字符串值。

每个 String 对象都有一个 `length` 属性，表示字符串中字符的数量。

String 类型提供以下方法来解析和操作字符串

#####  JavaScript 字符 

JavaScript 字符串由 16 位码元（code unit）组成，使用了两种 Unicode 编码混合的策略：UCS-2 和 UTF-16。

`charAt(index)`返回给定索引位置的字符

`charCodeAt()`方法可以查看指定码元的字符编码

`fromCharCode()` 方法用于根据给定的 UTF-16 码元创建字符串中的字符。

##### 字符串操作方法

`concat()`用于将一个或多个字符串拼接成一个新字符串。可以接收任意多个参数

提取子字符串方法

`slice()` 、` substr() `和 `substring()` 。其中

第一个参数表示子字符串**开始**的位置，第二个参数表示子字符串**结束**的位置。

对 `slice()`和 `substring()`而言，第二个参数是提取结束的位置（即该位置之前的字符会被提取出来）。

对 `substr()` 而言，第二个参数表示返回的子字符串**数量**。

##### 字符串位置方法

`indexOf()` 和 `lastIndexOf()` 。第一个参数表示要查找的子字符串，第二个可选参数，表示开始搜索的位置

`indexOf()`方法从字符串开头开始查找子字符串。

`lastIndexOf()`方法从字符串末尾开始查找子字符串。

##### 字符串包含方法

`startsWith()` : 从索引 0 开始的匹配项。返回布尔值。第二个参数表示开始搜索的位置。

`endsWith()` : 从索引`(string.length - substring.length)`开始的匹配项。返回布尔值。第二个参数示应该当作字符串末尾的位置，默认为字符串长度。

`includes()` : 检查整个字符串。返回布尔值。第二个参数表示开始搜索的位置。

##### 其他方法

`trim()`创建字符串的一个副本， 删除前、后所用的空格符。原始字符串不受影响。

`repeat()`接收一个整数参数，表示要将字符串复制多少次，然后返回拼接所有副本后的结果。

`padStart()`和`padEnd()`方法会复制字符串，如果小于指定长度，则在相应一边填充字符，直至满足长度条件

第一个参数是长度，第二个参数是可选的填充字符串，默认为空格。

```javascript
let stringValue = "foo"; 
 
console.log(stringValue.padStart(6));       // "   foo" 
console.log(stringValue.padStart(9, "."));  // "......foo" 
 
console.log(stringValue.padEnd(6));         // "foo   " 
console.log(stringValue.padEnd(9, "."));    // "foo......" 

// 如果提供了多个字符的字符串，则会将其拼接并截断以匹配指定长度。
console.log(stringValue.padStart(8, "bar")); // "barbafoo" 
console.log(stringValue.padStart(2));        // "foo" 
 
console.log(stringValue.padEnd(8, "bar"));   // "foobarba" 
console.log(stringValue.padEnd(2));          // "foo" 
```



#### 5.4 单例内置对象

代码开始执行时，全局上下文会存在两个内置对象：Global 和 Math。

浏览器讲Global实现为window对象。所有全局变量和函数都是Global对象的属性。

**1 Global**

**1.1 URL**

URL编码方法。两个。

`encodeURI()` ： 不会编码属于 URL 组件的特殊字符，比如冒号、斜杠、问号、井号

`encodeURIComponent()`：会编码它发现的所有非标准字符

对于的解码方法：

`encodeURI()` 和`encodeURIComponent()`

**1.2 eval() 方法**

该方法就是一个完整的 ECMAScript 解释器，它接收一个参数，即一个要执行的 ECMAScript（JavaScript）字符串。

eval() 定义的任何变量和函数都不会被提升。

在严格模式下，在 eval() 内部创建的变量和函数无法被外部访问。

**1.3 Global 对象属性**

Global 对象的属性包括

undefined 、 NaN 和 Infinity 等特殊值；

所有原生引用类型构造函数，比如 Object 和 Function ；

原始值包装类型构造函数， String，NUmber, Boolean

Date；

等等。

**1.4 window 对象**

浏览器将 window 对象实现为 Global对象的代理。因此，所有全局作用域中声明的变量和函数都变成了 window 的属性。

一种获取 Global 对象的方式是使用如下的代码： 

```javascript
let global = function() { 
  return this;  
}(); 

```



##### 2 Math

Math 对象作为保存数学公式、信息和计算的地方。

Math 对象上提供的计算要比直接在 JavaScript 实现的快得多，因为 Math 对象上的计算使用了 JavaScript 引擎中更高效的实现和处理器指令。但使用 Math 计算的问题是精度会因浏览器、操作系统、指令集和硬件而异。 

**2.1 Math对象属性**

Math对象的属性主要用于保存数学中的一些特殊值。

| 属性         | 说明                  |
| ------------ | --------------------- |
| Math.E       | 自然对数的基数 e 的值 |
| Math.LN10    | 10 为底的自然对数     |
| Math.LN2     | 2 为底的自然对数      |
| Math.LOG2E   | 以 2 为底 e 的对数    |
| Math.LOG10E  | 以 10 为底 e 的对数   |
| Math.PI      | π 的值                |
| Math.SQRT1_2 | 1/2 的平方根          |
| Math.SQRT2   | 的平方根              |

**2.2 方法**

`min()` 和 `max()` 方法用于确定一组数值中的最小值和最大值。

`Math.ceil()` 、 `Math.floor()` 、 `Math.round()`和 `Math.fround()`4 个方法用于把小数值舍入为整数.

`Math.random()` 方法返回一个 0~1 范围内的随机数，其中包含 0 但不包含 1。

其他方法