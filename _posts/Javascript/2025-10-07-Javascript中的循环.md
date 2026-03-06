---
title: Javascript中的循环
date: 2025-10-07
description: Javascript 中的循环
tags: [Javascript, javascript]
categories: [Javascript]
---
# Javascript 中的循环

### while 循环：

while 循环会在**指定条件为真时循环执行代码块**。

**语法：**

```javascript
while (条件)
{
  需要执行的代码
}
```

**实例：**

本例中的循环将继续运行，只要变量 i 小于 5：

```javascript
let [x, i] = ['', 0];
while (i<5){
  x=x + "该数字为" + i;
  i++;
}
console.log(x); //该数字为0  该数字为1  该数字为2  该数字为3  该数字为4
```

### do...while循环

**while 循环的变体**。该循环会**在检查条件是否为真之前执行一次代码块**，然后如果条件为真的话，就会重复这个循环。

**语法：**

```javascript
do
{
  需要执行的代码
}
while (条件);
```

**实例：**

**该循环至少会执行一次**，即使条件为 false 它也会执行一次，因为代码块会在条件被测试前执行：

```javascript
let [x, i] = ['', 0];
do{
  x=x + "该数字为 " + i;
  i++;
}
  while (i<5);
console.log(x); //该数字为 0该数字为 1该数字为 2该数字为 3该数字为 4
```

### for循环

1. for有三个表达式：①声明循环变量；②判断循环条件；③更新循环变量；三个表达式之间，用;分割，for循环三个表达式都可以省略，但是两个“;”缺一不可。
2. for循环的执行特点：先判断再执行，与while相同
3. for循环三个表达式都可以有多部分组成，第二部分多个判断条件用&& ||连接，第一三部分用逗号分割；

**实例**

```javascript
for (let num =1; num<=10; num++) {
  console.log(num); //1 2 3 4 5 6 7 8 9 10 
}
```

### for…in循环

1. for-in 循环主要用于**遍历对象**
2. for()中的格式：for(keys in zhangsan){}
3. keys表示obj对象的每一个键值对的键！！所有循环中，需要使用obj\[keys]来取到每一个值！！！

**实例**

```javascript
let obj = {a: 1, b: 2, c: 3};
for (let i in obj) {
  console.log('键名：', i);
  console.log('键值：', obj[i]);
}
/*
键名： a
键值： 1
键名： b
键值： 2
键名： c
键值： 3
*/
```

### for-of循环

ES6 借鉴 C++、Java、C# 和 Python 语言，引入了for...of循环，作为**遍历所有数据结构的统一的方法**。

一个数据结构只要部署了Symbol.iterator属性，就被视为具有iterator接口，就可以用for...of循环遍历它的成员。也就是说，for...of循环内部调用的是数据结构的Symbol.iterator方法。

for...of循环可以使用的范围包括\*\*数组、Set 和 Map 结构、某些类似数组的对象\*\*（比如arguments对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串。

JavaScript 原有的for...in循环，只能获得对象的键名，不能直接获取键值。ES6 提供for...of循环，允许遍历获得键值。

```javascript
let arr = ['a', 'b', 'c', 'd'];

for (let i in arr) {
  console.log(i); // 0 1 2 3
}

for (let i of arr) {
  console.log(i); //a b c d
}
```

更多请看 [这里](https://es6.ruanyifeng.com/#docs/iterator)

### map()循环

map() 方法**返回一个新数组**，数组中的元素为原始数组元素调用函数处理后的值。

> 注意： map() 不会对空数组进行检测；map() 不会改变原始数组。

**语法：**

```javascript
array.map(function(currentValue,index,arr), thisValue)
```

**实例**

```javascript
let numbers = [1, 2, 3];

let news = numbers.map((n)=>{
  return n + 1;
});
console.log(numbers); //[ 1, 2, 3 ]
console.log(news); //[ 2, 3, 4 ]
```

map方法接受一个函数作为参数。该函数调用时，map方法向它传入三个参数：**当前成员、当前位置和数组本身。**

```javascript
let numbers = [1, 2, 3];

let news = numbers.map((elem, index, arr)=>{
  return elem * index;

});
console.log(numbers); //[ 1, 2, 3 ]
console.log(news); //[ 0, 2, 6 ]
```

此外，map()循环还可以接受第二个参数，用来**绑定回调函数内部的this变量**，将回调函数内部的this对象，指向第二个参数，间接操作这个参数（一般是数组）。

```javascript
let arr = ['a', 'b', 'c'];

let news = [1, 2].map(function (e) {
  return this[e];
}, arr)；
console.log(news); //[ 'b', 'c' ]

//注意：这里用箭头函数就会出问题
```

上面代码通过map方法的第二个参数，将回调函数内部的this对象，指向arr数组。间接操作了数组arr; forEach同样具有这个功能。

### forEach循环

forEach方法与map方法很相似，也是对数组的所有成员依次执行参数函数。

但是，forEach方法**不返回值**，只用来操作数据。也就是说，如果数组遍历的目的是为了得到返回值，那么使用map方法，否则使用forEach方法。

forEach的用法与map方法一致，参数是一个函数，该函数同样接受三个参数：当前值、当前位置、整个数组。

```javascript
let arr = [1, 3, 5];

let news = arr.forEach((element, index, array)=>{
  console.log(index, element);
})
```

此外，forEach循环和map循环一样也可以用绑定回调函数内部的this变量，间接操作其它变量（参考上面的map()循环例子）。

### filter()过滤循环

filter方法用于过滤数组成员，**满足条件的成员组成一个新数组返回**。它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。

```javascript
let arr = [1, 2, 3, 4, 5];

let news = arr.filter((n)=>{
  return (n > 3);
});
console.log(arr); //[ 1, 2, 3, 4, 5 ]
console.log(news); //[ 4, 5 ]
```

```javascript
let arr = [0, 1, 'a', false];

let news = arr.filter(Boolean);
console.log(news); //[ 1, 'a' ]
```

filter方法的参数函数也可以接受三个参数：当前成员，当前位置和整个数组。

```javascript
let arr = [1, 2, 3, 4, 5];

let news = arr.filter((ele, index, arr)=>{
  return index % 2 === 0;
});
console.log(news); //[ 1, 3, 5 ]
```

此外，filter方法也可以接受第二个参数，用来绑定参数函数内部的this变量。

```javascript
let obj = { MAX: 3 };

let myFilter = function (item) {
  if (item > this.MAX) return true;
};

let arr = [2, 8, 3, 4, 1, 3, 2, 9];

let news = arr.filter(myFilter, obj);
console.log(news); //[ 8, 4, 9 ]
```

### some()，every()循环遍历，统计数组是否满足某个条件

这两个方法返回一个布尔值，表示判断数组成员是否符合某种条件。

它们接受一个函数作为参数，所有数组成员依次执行该函数。该函数接受三个参数：当前成员、当前位置和整个数组，然后返回一个布尔值。

* <code>**some**</code>方法是**只要一个成员的返回值是true，则整个some方法的返回值就是true，否则返回false**。

```javascript
let arr = [1, 2, 3, 4, 5];
let news = arr.some((elem, index, arr)=>{
  return elem >= 3;
});
console.log(news); //true
```

* 而<code>**every**</code>方法则相反，**所有成员的返回值都是true，整个every方法才返回true，否则返回false**。

```javascript
let arr = [1, 2, 3, 4, 5];
let news = arr.every((elem, index, arr)=>{
  return elem >= 3;
});
console.log(news); //false
```

两相比较，some()只要有一个是true，便返回true；而every()只要有一个是false，便返回false.

### reduce()，reduceRight()方法可依次处理数组的每个成员

reduce方法和reduceRight方法依次处理数组的每个成员，最终累计为一个值。

它们的差别是，reduce是从左到右处理（从第一个成员到最后一个成员），reduceRight则是从右到左（从最后一个成员到第一个成员），其他完全一样。

```javascript
let arr = [1, 2, 3, 4, 5];
let news = arr.reduce((a, b)=>{
  console.log(a, b);
  return a + b;
});
console.log(news); //15
// 1 2
// 3 3
// 6 4
// 10 5
// 15
```

reduce方法和reduceRight方法的**第一个参数都是一个函数**。该函数接受以下四个参数。

1. 累积变量，默认为数组的第一个成员
2. 当前变量，默认为数组的第二个成员
3. 当前位置（从0开始）
4. 原数组

* 这四个参数之中，只有前两个是必须的，后两个则是可选的。

如果要对累积变量**指定初值**，可以把它放在reduce方法和reduceRight方法的**第二个参数**。

```javascript
let arr = [1, 2, 3, 4, 5];
let news = arr.reduce((a, b)=>{
  return a + b;
},10);
console.log(news); //25
```

上面的第二个参数相当于设定了默认值，处理空数组时尤其有用，可避免一些空指针异常。

由于这两个方法会遍历数组，所以实际上还可以用来做一些遍历相关的操作。比如，**找出字符长度最长的数组成员。**

```javascript
function findLongest(entries) {
  return entries.reduce(function (longest, entry) {
    return entry.length > longest.length ? entry : longest;
  }, '');
}

console.log(findLongest(['aaa', 'bb', 'c'])); // "aaa"
```

上面代码中，reduce的参数函数会将字符长度较长的那个数组成员，作为累积值。这导致遍历所有成员之后，累积值就是字符长度最长的那个成员。

### Object.keys 遍历对象的属性

`Object.getOwnPropertyNames`方法与`Object.keys`类似：

也是**接受一个对象作为参数，返回一个数组，包含了该对象自身的所有属性名**。但它能返回不可枚举的属性。

```javascript
let a = ['Hello', 'World'];

let new1 = Object.keys(a);
let new2 = Object.getOwnPropertyNames(a);
console.log(new1); // ["0", "1"]
console.log(new2); // ["0", "1", "length"]
```

上面代码中，数组的length属性是不可枚举的属性，所以只出现在Object.getOwnPropertyNames方法的返回结果中。

由于 JavaScript 没有提供计算对象属性个数的方法，所以可以用这两个方法代替。

```javascript
let obj = {
  p1: 123,
  p2: 456
};

let new1 = Object.keys(obj).length;
let new2 = Object.getOwnPropertyNames(obj).length;
console.log(new1); // 2
console.log(new2); // 2
```

### 比较

#### 一、map()，foreach，filter循环的共同之处：

1. foreach，map，filter 循环中途是**无法停止**的，总是会将所有成员遍历完。
2. 他们都可以接受第二个参数，用来绑定回调函数内部的this变量，将回调函数内部的this对象，指向第二个参数，间接操作这个参数（一般是数组）。

#### 二、map()循环和forEach循环的不同：

forEach循环没有返回值；map，filter循环有返回值。

#### 三、map()循环和filter()循环都会跳过空位，for和while不会

```javascript
var f = function (n) {
  return 'a'
};

[1, undefined, 2].map(f) // ["a", "a", "a"] 
[1, null, 2].map(f) // ["a", "a", "a"]
[1, , 2].map(f) // ["a", , "a"]
```

上面代码中，map方法不会跳过undefined和null，但是会跳过空位。forEach方法也会跳过数组的空位，这里就不举例了。

#### 四、some()和every():

some()只要有一个是true，便返回true；而every()只要有一个是false，便返回false.

#### 五、reduce()，reduceRight()：

reduce是从左到右处理（从第一个成员到最后一个成员），reduceRight则是从右到左（从最后一个成员到第一个成员）。

#### 六、Object对象的两个遍历Object.keys与Object.getOwnPropertyNames：

他们都是遍历对象的属性，也是接受一个对象作为参数，返回一个数组，包含了该对象自身的所有属性名。但Object.keys不能返回不可枚举的属性；Object.getOwnPropertyNames能返回不可枚举的属性。


