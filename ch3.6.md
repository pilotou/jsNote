### 3.6语句

#### 1.for-in 语句

```javascript
for (property in expression) statement 
```

for-in 语句是一种严格的迭代语句，用于枚举对象中的非符号键属性.

控制语句中的 const 也不是必需的。但为了确保这个局部变量不被修改，推荐使用 const 。 

ECMAScript 中对象的属性是**无序**的，因此 for-in 语句**不能保证返回对象属性的顺序**。

#### 2.for-of 语句

for-of 语句是一种严格的迭代语句，用于遍历可迭代对象的元素

```javascript
for (property of expression) statement 
```

for-of 循环会按照可迭代对象的 next() 方法产生值的顺序迭代元素。如果尝试迭代的变量不支持迭代，则 for-of 语句会抛出错误。 

#### 3.标签语句

标签语句用于给语句加标签，语法如下： 
```label: statement ```
下面是一个例子： 

```javascript
start: for (let i = 0; i < count; i++) { 
  console.log(i);  
} 
```

在这个例子中， start 是一个标签，可以在后面通过 break 或 continue 语句引用。标签语句的典型应用场景是嵌套循环。

#### 4.break 和 continue 语句

break 语句用于立即退出循环，强制执行循环后的下一条语句。

continue 语句也用于立即退出循环，但会再次从循环顶部开始执行。

通过组合使用标签语句和 break 、 continue 能实现复杂的逻辑，但也容易出错。

#### 5.with 语句

with 语句的用途是将代码作用域设置为特定的对象，其语法是： 

```javascript
with (expression) statement; 
```

使用 with 语句的主要场景是针对一个对象**反复操作**，这时候将代码作用域设置为该对象能提供便利，如下面的例子所示： 

```javascript
let qs = location.search.substring(1); 
let hostName = location.hostname; 
let url = location.href; 

// 上面代码中的每一行都用到了 location 对象。如果使用 with 语句，就可以少写一些代码： 
with(location) {  
  let qs = search.substring(1); 
  let hostName = hostname; 
  let url = href; 
} 
```

严格模式不允许使用 with 语句，否则会抛出错误。

**警告**  由于 with 语句影响性能且难于调试其中的代码，通常不推荐在产品代码中使用 with语句。 



#### 6.switch 语句

ECMAScript 中 switch语句跟 C 语言中 switch 语句的语法非常相似。

switch 语句在比较每个条件的值时会使用全等操作符，因此不会强制转换数据类型。