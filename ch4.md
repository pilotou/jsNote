# 第四章 变量、作用域与内存

JavaScript 变量是松散类型的，变量不过就是特定时间点一个特定值的名称而已。变量的值和数据类型在脚本生命期内可以改变

### 4.1 原始值与引用值

ECMAScript 变量可以包含两种不同类型的数据：原始值和引用值

原始值(primitive value)：就是最简单的数据。JavaScript有6中种原始值：Undefined 、 Null 、 Boolean 、 Number 、 String 和 Symbol。保存原始值的变量是按值（by value）访问的，因为我们操作的就是存储在变量中的实际值。 

引用值(reference value)： 由多个值构成的对象。保存引用值的变量是按引用（by reference）访问的。

> 引用值是保存在内存中的对象，JavaScript 不允许直接访问内存位置，因此也就不能直接操作对象所在的内存空间在操作对象时，实际上操作的是对该对象的引用（reference）而非实际的对象本身为此，保存引用值的变量是按引用（by reference）访问的。 

#### 1 动态属性

引用值可以随时添加、修改和删除其属性和方法。

原始值不能有属性，但是给原始值添加属性不会报错。

```javascript
// 引用值
let person = new Object(); 
person.name = "Nicholas"; 
console.log(person.name); // "Nicholas" 
//原始值
let name = "Nicholas"; 
name.age = 27; 
console.log(name.age);  // undefined 
```

原始类型的初始化可以只使用**原始字面量形式**。如果使用的是 new 关键字，则 JavaScript 会创建一个 **Object** 类型的实例，但其行为类似原始值。

```javascript
let name1 = "Nicholas"; 
let name2 = new String("Matt"); 
name1.age = 27; 
name2.age = 26; 
console.log(name1.age);    // undefined 
console.log(name2.age);    // 26 
console.log(typeof name1); // string 
console.log(typeof name2); // object 
```

#### 2 复制值

把一个原始值从一个变量赋给另一个变量时，原始值会被复制到新变量的位置

把一个引用值从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在的位置，但是这里复制的值实际上是一个**指针**

#### 3 传递参数

ECMAScript 中所有函数的参数都是**按值传递**的。

例如

```javascript
function setName(obj) { 
  obj.name = "Nicholas";  
  obj = new Object(); 
  obj.name = "Greg"; 
} 
 
let person = new Object(); 
setName(person); 
console.log(person.name);  // "Nicholas" 
```

如果 person 是按引用传递的，那么 person 的指针改为指向 name 为 "Greg" 的对象。可是，当我们再次访问 person.name 时，它的值是 "Nicholas" ，这表明函数中参数的值改变之后，原始的引用仍然没变。

#### 4 确定类型

`typeof` 操作符最适合用来判断一个变量是否为原始类型。但是 `typeof null`会返回一个`object`

对于引用值，可以用是`instanceof`操作符，语法如下

```javascript
result = variable instanceof constructor 
```

例如

```javascript
console.log(person instanceof Object);  // 变量 person 是 Object 吗？ 
console.log(colors instanceof Array);   // 变量 colors 是 Array 吗？ 
console.log(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？ 
```

所有引用值都是 Object 的实例。`typeof` 操作符在用于检测函数时也会返回 "function" 。



### 4.2 执行上下文与作用域

变量或函数的**上下文(context)**决定了它们可以访问哪些数据，以及它们的行为。每个上下文都有一个关联的变量对象（variable  object），而这个上下文中定义的所有变量和函数都存在于这个对象上。全局上下文是最外层的上下文。

在浏览器中，全局上下文就是我们常说的 window 对象，所有通过 var 定义的全局变量和函数都会成为 window 对象的属性和方法。使用 let 和 const 的顶级声明不会定义在全局上下文中。

内部上下文可以通过作用域链访问外部上下文中的一切，反之则不可以。

某些语句会导致在作用域链前端临时添加一个上下文，这个上下文在代码执行后会被删除。这种现象叫做作用**域链增强**。通常出现在两种情况

* try/catch 语句的catch块
* with语句

var声明的变量会被自动添加到最接近的上下文。

var 声明会被拿到函数或全局作用域的顶部，位于作用域中所有代码之前。这个现象叫作“提升”（hoisting）。例如

```javascript
var name = "Jake"; 
 
// 等价于： 
 
name = 'Jake'; 
var name; 
```

let的作用域是块级的。块级作用域由最近的一对包含花括号 {} 界定。

const 声明的变量必须同时初始化为某个值。一经声明，在其生命周期的任何时候都不能再重新赋予新值。 

### 4.3 垃圾回收

基本思路：：确定哪个变量不会再使用，然后释放它占用的内存。

在浏览器的发展史上，用到过两种主要的标记策略：**标记清理(mark-and-sweep)**和**引用计数(reference counting)**

