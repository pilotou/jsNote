## 变量

有3个关键字可以声明变量：`var`、`const`和`let`。`var` 在ECMAScript 的所有版本中都可以使用，而 `const`和 `let` 只能在 ECMAScript 6 及更晚的版本中使用。 

#### 1. var 关键字

##### 1.1 var声明作用域

使用 `var`操作符定义的变量会成为包含它的函数的局部变量。比如，使用`var`在一个函数内部定义一个变量，就意味着该变量将在函数退出时被销毁：

```javascript
function test() {  
  var message = "hi"; // 局部变量 
} 
test(); 
console.log(message); // 出错！
```

不过，在函数内定义变量时省略 `var` 操作符，可以创建一个全局变量： 

```javascript
function test() { 
  message = "hi";     // 全局变量 
} 
test(); 
console.log(message); // "hi" 
```

但是不推荐这样做。在严格模式下会导致抛出`ReferenceError`。

##### 1.2 var 声明提升

下面的代码不会报错

```javascript
function foo() {  
  console.log(age); 
  var age = 26; 
} 
foo();  // undefined 
```

因为 ECMAScript 运行时把它看成等价于如下代码： 

```javascript
function foo() { 
  var age; 
  console.log(age); 
  age = 26; 
} 
foo();  // undefined 
```

这就是所谓的“**提升**”（hoist），也就是把所有变量声明都拉到函数作用域的顶部。

反复多次使用 `var` 声明同一个变量也没有问题：

```javascript
function foo() { 
  var age = 16; 
  var age = 26; 
  var age = 36; 
  console.log(age); 
} 
foo();  // 36 
```



#### 2. let声明

`let` 跟 `var` 的作用差不多，但有着非常重要的区别。最明显的区别是，` let` 声明的范围是**块作用域**，而 `var` 声明的范围是**函数作用域**。 

```javascript
if (true) { 
  var name = 'Matt'; 
  console.log(name); // Matt 
} 
console.log(name);   // Matt 
```

```javascript
if (true) { 
  let age = 26; 
  console.log(age);   // 26 
} 
console.log(age);     // ReferenceError: age 没有定义 
```

age 变量之所以不能在 if 块外部被引用，是因为它的作用域仅**限于该块内部**。**块作用域是函数作用域的子集**，因此适用于`var`的作用域限制同样也适用于`let`。 

##### 2.1暂时性死区

`let` 与 `var` 的另一个重要的区别，就是 `let` 声明的变量不会在作用域中被提升。 

```javascript
// name 会被提升 
console.log(name); // undefined 
var name = 'Matt'; 
// age 不会被提升 
console.log(age); // ReferenceError：age 没有定义 
let age = 26;  
```

##### 2.2全局声明

与 `var` 关键字不同，使用 `let` 在全局作用域中声明的变量不会成为 window 对象的属性（ `var` 声明的变量则会）。

```javascript
var name = 'Matt'; 
console.log(window.name); // 'Matt' 
let age = 26; 
console.log(window.age);  // undefined 
```

 不过， `let` 声明仍然是在全局作用域中发生的，相应变量会在页面的生命周期内存续

##### 2.3 条件声明

在使用 `var` 声明变量时，由于声明会被提升，JavaScript 引擎会自动将多余的声明在作用域顶部合并为一个声明。因为 `let` 的作用域是块，所以不可能检查前面是否已经使用 let 声明过同名变量，同时也就不可能在没有声明的情况下声明它。 

##### 2.4 for循环中的let声明

使用var时，for循环定义的迭代变量会渗透到循环体外部

```javascript
for (var i = 0; i < 5; ++i) { 
    setTimeout(() => console.log(i), 0) 
} 
// 你可能以为会输出 0、1、2、3、4 
// 实际上会输出 5、5、5、5、5 
```

改成使用 let 之后，这个问题就消失了，因为迭代变量的作用域仅限于 for 循环块内部： 

```js
for (let i = 0; i < 5; ++i) { 
    setTimeout(() => console.log(i), 0) 
} 
// 会输出 0、1、2、3、4 
```

#### 3. const声明

`const` 的行为与 `let` 基本相同，唯一一个重要的区别是用它声明变量时**必须同时初始化变量**，且尝试修改 `const` 声明的变量会导致运行时错误。 

const的作用域也是块

```js
const name = 'Matt'; 
if (true) {  
  const name = 'Nicholas'; 
} 
console.log(name); // Matt 
```

const 声明的限制只适用于它指向的变量的引用。换句话说，如果 const 变量引用的是一个对象，那么修改这个对象内部的属性并不违反 const 的限制。 

```js
const person = {}; 
person.name = 'Matt';  // ok 
```

#### 4. 声明的最佳风格

* 不使用var
* const优先，let次之





### 数据类型

ECMAScript 有 6 种简单数据类型（也称为原始类型）： Undefined 、 Null 、 Boolean 、 Number 、String 和 Symbol 。还有一种复杂数据类型叫 Object （对象）

typeof操作符用于确定变量的数据类型。

typeof null会返回object。因为特殊值 null 被认为是一个**对空对象的引用**。 

##### 1.Undefined类型

Undefiened 类型只有一个值，就是undefined。当使用 var 或 let 声明了变量但没有初始化时，就相当于给变量赋予了 undefined 值。

对于声明了但是没有初始化的变量，调用 `typeof` 时，返回的结果是 "undefined" ；

对于未声明的变量，调用 `typeof` 时，返回的结果也是 "undefined" ；

但是，如果调用console.log，则会出错。

```javascript
let message;    // 这个变量被声明了，只是值为 undefined 

// 确保没有声明过这个变量 
// let age 

console.log(message); // "undefined" 
console.log(age);     // 报错 

console.log(typeof message); // "undefined" 
console.log(typeof age);     // "undefined" 
```



##### 2.null类型

Null 类型同样只有一个值，即特殊值 `null` 。

逻辑上讲， `null` 值表示一个**空对象指针**，这也是给`typeof` 传一个 null 会返回 "object" 的原因。

在定义将来要保存对象值的变量时，建议使用 null 来初始化，不要使用其他值。

`undefined` 值是由 `null` 值派生而来的，因此 ECMA-262 将它们定义为表面上相等，如下面的例子所示： 

```javascript
console.log(null == undefined);  // true
```

##### 3.Boolean类型

有两个字面值： true 和 false 。
这两个布尔值不同于数值，因此 true 不等于 1， false 不等于 0。

##### 4.Number类型

表示八进制，第一个数字必须时0，如果字面量中包含的数字超出了应有的范围，就会忽略前缀的零，后面的数字序列会被当成十进制数。八进制字面量在严格模式下是无效的。

```javascript
let octalNum1 = 070;  // 八进制的 56 
let octalNum2 = 079;  // 无效的八进制值，当成 79 处理 
let octalNum3 = 08;   // 无效的八进制值，当成 8 处理 
```

表示16进制。数值前缀 0x （区分大小写），然后是十六进制数字（0~9 以及 A~F）。十六进制数字中的字母大小写均可。

```javascript
let hexNum1 = 0xA;   // 十六进制 10 
let hexNum2 = 0x1f;  // 十六进制 31
```

###### 4.1 浮点值

* 要定义浮点值，数值中必须包含小数点，而且小数点后面必须至少有一个数字。

* 存储浮点值使用的内存空间是存储整数值的**两倍**，所以 ECMAScript 总是想方设法把值转换为整数。

* 在小数点后面没有数字的情况下，数值就会变成整数。

* 类似地，如果数值本身就是整数，只是小数点后面跟着 0（如 1.0），那它也会被转换为整数。

对于非常大或非常小的数值，浮点值可以用科学记数法来表示。ECMAScript 中科学记数法的格式要求是一个数值（整数或浮点数）后跟一个大写或小写的字母 e，再加上一个要乘的 10 的多少次幂

```javascript
let floatNum = 3.125e7; // 等于 31250000 
```

默认情况下，ECMAScript 会将小数点后至少包含 6 个零的浮点值转换为科学记数法（例如，0.000 000 3 会被转换为 3e-7）。

浮点值的精确度最高可达 17 位小数，但在算术计算中远不如整数精确。例如，0.1 加 0.2 得到的不是 0.3，而是 0.300 000 000 000 000 04。如果两个数值分别是 0.05 和 0.25，或者 0.15 和 0.15，那没问题.

之所以存在这种舍入错误，是因为使用了 IEEE 754 数值，这种错误并非 ECMAScript
所独有。其他使用相同格式的语言也有这个问题。 

###### 4.2 值的范围

ECMAScript 可以表示的最小数值保存在 Number.MIN_VALUE 中，这个值在多数浏览器中是 5e-324；可以表示的最大数值保存在Number.MAX_VALUE 中，这个值在多数浏览器中是 1.797 693 134 862 315 7e+308。

如果某个计算得到的数值结果超出了 JavaScript 可以表示的范围，那么这个数值会被自动转换为一个特殊的 Infinity （无穷）值。任何无法表示的负数以 -Infinity （负无穷大）表示，任何无法表示的正数以 Infinity （正无穷大）表示。 
如果计算返回正 Infinity 或负 Infinity ，则该值将不能再进一步用于任何计算。用`isFinite()` 函数可以确定一个值是不是有限大

###### 4.3 NaN

特殊的数值 NaN的意思是“不是数值”（Not a Number），用于表示本来要返回数值的操作失败了（而不是抛出错误）。比如，用 0 除任意数值在其他语言中通常都会导致错误，从而中止代码执行。但在 ECMAScript 中，0、+0 或-0 相除会返回 NaN ： 

```javascript
console.log(0/0);    // NaN 
console.log(-0/+0);  // NaN 
```

NaN的属性

* 任何涉及 NaN 的操作始终返回 NaN （如 NaN/10 ），

*  NaN 不等于包括 NaN 在内的任何值，例如

  ```javascript
  console.log(NaN == NaN); // false 
  ```

isNaN()函数可以用于判断这个参数是否”不是数值“。把一个值传给 isNaN() 后，该函数会尝试把它转换为数值。某些非数值的值可以直接转换成数值，如字符串 "10" 或布尔值。任何不能转换为数值的值都会导致这个函数返回true 。

```javascript
console.log(isNaN(NaN));     // true 
console.log(isNaN(10));      // false，10 是数值 
console.log(isNaN("10"));    // false，可以转换为数值 10 
console.log(isNaN("blue"));  // true，不可以转换为数值 
console.log(isNaN(true));    // false，可以转换为数值 1 
```

###### 4.4 数值转换

有 3 个函数可以将非数值转换为数值： Number() 、 parseInt() 和 parseFloat() 。Number() 是转型函数，可用于任何数据类型。后两个函数主要用于将字符串转换为数值。

**4.4.1Number()**

转换规则：

* 布尔值， true 转换为 1， false 转换为 0。
* 数值，直接返回。
* null ，返回 0。
* undefined ，返回 NaN 。
* 字符串：
  * 如果字符串包含数值字符，包括数值字符前面带加、减号的情况，则转换为一个十进制数值。因此， Number("1") 返回 1， Number("123") 返回 123， Number("011") 返回 11（忽略前面的零）。
  * 如果字符串包含有效的浮点值格式如 "1.1" ，则会转换为相应的浮点值（同样，忽略前面的零）。 
  * 如果字符串包含有效的十六进制格式如 "0xf" ，则会转换为与该十六进制值对应的十进制整数值。
  * 如果是空字符串（不包含字符），则返回 0。 
  * 如果字符串包含除上述情况之外的其他字符，则返回 NaN 。  
* 对象，调用 `valueOf()` 方法，并按照上述规则转换返回的值。如果转换结果是 NaN ，则调用`toString()` 方法，再按照转换字符串的规则转换。 

**4.4.2 parseInt()**

通常在需要得到整数时可以优先使用 parseInt() 函数。 parseInt() 函数更专注于字符串是否包含数值模式。

字符串最前面的空格会被忽略，从第一个非空格字符开始转换。

如果第一个字符不是数值字符、加号或减号， parseInt() 立即返回 NaN 。这意味着空字符串也会返回 NaN （这一点跟 Number() 不一样，它返回 0）。

如果第一个字符是数值字符、加号或减号，则继续依次检测每个字符，直到字符串末尾，或碰到非数值字符。

例子

```javascript
let num1 = parseInt("1234blue");  // 1234 
let num2 = parseInt("");          // NaN 
let num3 = parseInt("0xA");       // 10，解释为十六进制整数 
let num4 = parseInt(22.5);        // 22 
let num5 = parseInt("70");        // 70，解释为十进制值 
let num6 = parseInt("0xf");       // 15，解释为十六进制整数 
```

parseInt() 也接收第二个参数，用于指定底数（进制数）

```javascript
let num = parseInt("0xAF", 16); // 175 
//事实上，如果提供了十六进制参数，那么字符串前面的 "0x" 可以省掉： 
let num1 = parseInt("AF", 16);  // 175 
let num2 = parseInt("AF");      // NaN 
```

**4.4.3 parseFloat()**

parseFloat() 函数的工作方式跟 parseInt() 函数类似，都是从位置 0 开始检测每个字符。同样，
它也是解析到**字符串末尾**或者解析到一个**无效的浮点数值字符**为止

parseFloat() 函数的另一个不同之处在于，它**始终忽略字符串开头**的零。这个函数能识别前面讨
论的所有浮点格式，以及十进制格式（开头的零始终被忽略）。十六进制数值始终会返回 0。因为parseFloat() **只解析十进制值**，因此不能指定底数。

如果字符串表示整数（没有小数点或者小数点后面只有一个零），则 parseFloat() 返回整数

```javascript
let num1 = parseFloat("1234blue");  // 1234，按整数解析 
let num2 = parseFloat("0xA");       // 0 
let num3 = parseFloat("22.5");      // 22.5 
let num4 = parseFloat("22.34.5");   // 22.34 
let num5 = parseFloat("0908.5");    // 908.5 
let num6 = parseFloat("3.125e7");   // 31250000 
let num7 = parseFloat("3");  // 3
let num8 = parseFloat("3.0");  // 3
```

#### 5. String类型

String （字符串）数据类型表示零或多个 16 位 Unicode 字符序列。字符串可以使用双引号（"）、单引号（'）或反引号（`）标示，

字符串的长度可以通过其 length 属性获取。

ECMAScript 中的字符串是不可变的（immutable），意思是一旦创建，它们的值就不能变了。要修改某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量

转换为字符串：

* `toString()` 方法。这个方法唯一的用途就是返回当前值的字符串等价物。null 和 undefined 值没有 `toString()` 方法。

* `String()`。`String()` 函数遵循如下规则：

   如果值有 `toString()` 方法，则调用该方法（不传参数）并返回结果。 
   如果值是 null ，返回 "null" 。 
   如果值是 undefined ，返回 "undefined" 。 

**模板字面量**

模板字面量保留换行字符，可以跨行定义字符串，用反引号表示。

模板字面量会保持反引号内部的空格。

```javascript
let myMultiLineString = 'first line\nsecond line'; 
let myMultiLineTemplateLiteral = `first line 
second line`; 
 
console.log(myMultiLineString); 
// first line 
// second line" 
 
console.log(myMultiLineTemplateLiteral); 
// first line 
// second line 
 
console.log(myMultiLineString === myMultiLinetemplateLiteral); // true 
```

模板字面量在定义模板时特别有用。

```javascript
let pageHTML = `  
<div> 
  <a href="#"> 
    <span>Jake</span> 
  </a> 
</div>`; 
```

#### 6.Symbol 类型

* 符号是**原始值**，且符号实例是**唯一**、**不可变**的。
* 符号的用途是确保对象属性使用**唯一标识符**，不会发生属性冲突的危险

6.1基本用法

使用Symbol()函数初始化

```javascript
let sym = Symbol(); 
console.log(typeof sym); // symbol 
// 可以传入一个字符串参数作为对符号的描述（description）
let genericSymbol = Symbol(); 
let otherGenericSymbol = Symbol(); 
 
let fooSymbol = Symbol('foo'); 
let otherFooSymbol = Symbol('foo'); 
 
console.log(genericSymbol == otherGenericSymbol);  // false 
console.log(fooSymbol == otherFooSymbol);          // false 

// 符号没有字面量语法
let genericSymbol = Symbol(); 
console.log(genericSymbol);  // Symbol() 
let fooSymbol = Symbol('foo'); 
console.log(fooSymbol);      // Symbol(foo); 
```

要创建 Symbol() 实例并将其用作对象的新属性，就可以保证它不会覆盖已有的对象属性，无论是符号属性还是字符串属性.

Symbol() 函数不能与 new 关键字一起作为构造函数使用

#### 7.Object类型

对象其实就是一组数据和功能的集合。对象通过 new 操作符后跟对象类型的名称来创建。

每个 Object 实例都有如下属性和方法：

* `constructor` ：用于创建当前对象的函数。在前面的例子中，这个属性的值就是 Object() 函数。 
* `hasOwnProperty(propertyName) `：用于判断当前对象实例（不是原型）上是否存在给定的属性。要检查的属性名必须是字符串（如 o.hasOwnProperty("name") ）或符号。 
* `isPrototypeOf(object)` ：用于判断当前对象是否为另一个对象的原型。（第 8 章将详细介绍原型。）
* `propertyIsEnumerable(propertyName)` ：用于判断给定的属性是否可以使用（本章稍后讨论的） for-in 语句枚举。与 hasOwnProperty() 一样，属性名必须是字符串。 
* `toLocaleString() `：返回对象的字符串表示，该字符串反映对象所在的本地化执行环境。 
* `toString()` ：返回对象的字符串表示。 
* `valueOf()` ：返回对象对应的字符串、数值或布尔值表示。通常与 `toString()` 的返回值相同。 

### 操作符

#### 1.一元操作符

前缀操作符: 前缀递增操作符`++`, 前缀递减操作符`--`，变量的值在语句被求值之前改变

```javascript
let age = 29; 
let anotherAge = --age + 2; 
 
console.log(age);         // 28 
console.log(anotherAge);  // 30
```

后缀操作符在语句被求值后才发生。

```javascript
let num1 = 2; 
let num2 = 20; 
let num3 = num1-- + num2; 
let num4 = num1 + num2; 
console.log(num3);  // 22 
console.log(num4);  // 21 
```

#### 2.位操作符

ECMAScript中的所有数值都以 IEEE  754  64 位格式存储，但位操作并不直接应用到 64 位表示，而是先把值转换为32 位整数，再进行位操作，之后再把结果转换为 64 位。故只需要考虑 32 位整数即可。

正值以有符号整数表示，前 31 位表示整数值，第 32 位表示数值的符号

负值以一种称为二补数（或补码）的二进制编码存储。补码的计算步骤：

(1) 确定绝对值的二进制表示（如，对于18，先确定 18 的二进制表示）； 
(2) 找到数值的一补数（或反码），换句话说，就是每个 0 都变成 1，每个 1 都变成 0； 
(3) 给结果加 1。 

**按位`非`~**

按位非的最终效果是对数值取反并减 1

```javascript
let num1 = 25;      // 二进制 00000000000000000000000000011001 
let num2 = ~num1;   // 二进制 11111111111111111111111111100110 
console.log(num2);  // -26 
```

**按位与`&`**

按位与就是将两个数的每一个位对齐，然后基于真值表中的规则，对每一位执行相应的与操作.

```javascript
let result = 25 & 3;  
console.log(result); // 1 
```

计算过程如下

![1619168662429](image\ch3-1.png)

所以结果为1

**按位或`|`**

与按位与相似。

**按位异或 `^`**

与按位与相似。

**左移`<<`**

注意，左移会保留它所操作数值的符号。

**有符号右移`>>`**

将数值的所有 32 位都向右移，同时保留符号（正或负）。右移后左边的空位会用符号位的值来填充。

**无符号右移`>>>`**

与有符号右移不同，无符号右移会给**空位补 0**，而不管符号位是什么。

#### 3.布尔操作符

逻辑非`!`， 逻辑或`||`，逻辑与`&&`

#### 4.乘性操作符 

ECMAScript 定义了 3 个乘性操作符：乘法`*`、除法`/`和取模`%`

#### 5.指数操作符 

ECMAScript 7 新增了指数操作符`**`，与` Math.pow()` 结果是一样的： 

#### 6.加性操作符

如果两个操作数都是数值，加法操作符执行加法运算并根据如下规则返回结果： 
 如果有任一操作数是 NaN ，则返回 NaN ； 
 如果是 Infinity 加 Infinity ，则返回 Infinity ； 
 如果是 -Infinity 加 -Infinity ，则返回 -Infinity ； 
 如果是 Infinity 加 -Infinity ，则返回 NaN ； 
 如果是 +0 加 +0 ，则返回 +0 ； 
 如果是 -0 加 +0 ，则返回 +0 ； 
 如果是 -0 加 -0 ，则返回 -0 。 
如果有一个操作数是字符串，则要应用如下规则： 
 如果两个操作数都是字符串，则将第二个字符串拼接到第一个字符串后面； 
 如果只有一个操作数是字符串，则将另一个操作数转换为**字符串**，再将两个字符串拼接在一起。 

#### 7.相等操作符

**等于`==`和不等于`!=`**

这两个操作符都会先进行类型转换（通常称为**强制类型转换**）再确定操作数是否相等。

 如果任一操作数是布尔值，则将其转换为数值再比较是否相等。 false 转换为 0， true 转换
为 1。 
 如果一个操作数是字符串，另一个操作数是数值，则尝试将字符串转换为数值，再比较是否
相等。 
 如果一个操作数是对象，另一个操作数不是，则调用对象的 valueOf() 方法取得其原始值，再
根据前面的规则进行比较。 

  null 和 undefined 相等。 
  null 和 undefined 不能转换为其他类型的值再进行比较。 
 如果有任一操作数是 NaN ，则相等操作符返回 false ，不相等操作符返回 true 。记住：即使两
个操作数都是 NaN ，相等操作符也返回 false ，因为按照规则， NaN 不等于 NaN 。 
 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，
则相等操作符返回 true 。否则，两者不相等。 

**全等`===`和不全等`!==`**

只有两个操作数在不转换的前提下相等才返回 true

#### 8.条件操作符`?`

语法和Java中的一样

```javascript
variable = boolean_expression ? true_value : false_value; 
```

