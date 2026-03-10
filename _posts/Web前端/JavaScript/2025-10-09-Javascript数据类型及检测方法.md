---
title: Javascript数据类型及检测方法
date: 2025-10-09
description: Javascript数据类型及检测方法
tags: [Javascript, javascript]
categories: [Web前端, JavaScript]
---
# Javascript数据类型及检测方法

Javascript数据类型



在 ECMAScript 规范中，共定义了 6 种数据类型（加一个ES6新类型共7种），分为 **基本类型** 和 **引用类型** 两大类，如下所示：



> 基本类型：String、Number、Boolean、Undefined、Null、Symbol（ES6新类型，最下面扩展有介绍）  
引用类型：Object



基本类型也称为简单类型，由于其占据空间固定，是简单的数据段，为了便于提升变量查询速度，将其存储在栈中，即按值访问。



引用类型也称为复杂类型，由于其值的大小会改变，所以不能将其存放在栈中，否则会降低变量查询速度，因此，其值存储在堆(heap)中，而存储在变量处的值，是一个指针，指向存储对象的内存处，即按址访问。引用类型除 Object 外，还包括 Function 、Array、RegExp、Date 等等。



### 检测方法


#### typeof


```javascript
console.log(typeof 'hello');// string 有效
console.log(typeof 123);// number 有效
console.log(typeof false);// boolean 有效
console.log(typeof undefined);// undefined 有效
console.log(typeof Symbol());// symbol 有效
console.log(typeof null);// object 无效
console.log(typeof [1,2,3]);// object 无效
console.log(typeof new Function());// function 有效
console.log(typeof new Date());// object 无效
console.log(typeof new RegExp());// object 无效
```



有些时候，typeof 操作符会返回一些令人迷惑但技术上却正确的值：



+ 对于基本类型，除 null 以外，均可以返回正确的结果。
+ 对于引用类型，除 function 以外，一律返回 object 类型。
+ 对于 null ，返回 object 类型。
+ 对于 function 返回  function 类型。



其中，null 有属于自己的数据类型 Null ， 引用类型中的 数组、日期、正则 也都有属于自己的具体类型，而 typeof 对于这些类型的处理，只返回了处于其原型链最顶端的 Object 类型，没有错，但不是我们想要的结果。



#### instanceof


instanceof 是用来判断 A 是否为 B 的实例，表达式为：A instanceof B，如果 A 是 B 的实例，则返回 true,否则返回 false。 在这里需要特别注意的是：instanceof 检测的是原型，我们用一段伪代码来模拟其内部执行过程：



```javascript
instanceof (A,B) = {
    var L = A.__proto__;
    var R = B.prototype;
    if(L === R) {
        // A的内部属性 __proto__ 指向 B 的原型对象
        return true;
    }
    return false;
}
```



从上述过程可以看出，当 A 的 **proto** 指向 B 的 prototype 时，就认为 A 就是 B 的实例，我们再来看几个例子：



```javascript
console.log([1,2,3] instanceof Array);// true
console.log({} instanceof Object);// true
console.log(new Date() instanceof Date);// true
function Person(){}
console.log(new Person() instanceof Person);// true
console.log([1,2,3] instanceof Object);// true
console.log(new Date() instanceof Object);// true
console.log(new Person instanceof Object);// true
```



我们发现，虽然 instanceof 能够判断出 [ ] 是Array的实例，但它认为 [ ] 也是Object的实例，为什么呢？



我们来分析一下 [ ]、Array、Object 三者之间的关系：



从 instanceof 能够判断出 [ ].**proto**  指向 Array.prototype，而 Array.prototype.**proto** 又指向了Object.prototype，最终 Object.prototype.**proto** 指向了null，标志着原型链的结束。因此，[]、Array、Object 就在内部形成了一条原型链：



![1574647141027-5031299d-1c11-4d38-8bdf-14c428c1ce79.png](/assets/img/posts/Web前端/JavaScript/1574647141027-5031299d-1c11-4d38-8bdf-14c428c1ce79-922484.png)



从原型链可以看出，[] 的 **proto**  直接指向Array.prototype，间接指向 Object.prototype，所以按照 instanceof 的判断规则，[] 就是Object的实例。依次类推，类似的 new Date()、new Person() 也会形成一条对应的原型链 。因此，instanceof 只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型。



instanceof 操作符的问题在于，它假定只有一个全局执行环境。如果网页中包含多个框架，那实际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的构造函数。如果你从一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别具有各自不同的构造函数。



```javascript
var iframe = document.createElement('iframe');
document.body.appendChild(iframe);
xArray = window.frames[0].Array;
var arr = new xArray(1,2,3); // [1,2,3]
arr instanceof Array; // false
```



针对数组的这个问题，ES5 提供了 Array.isArray() 方法 。该方法用以确认某个对象本身是否为 Array 类型，而不区分该对象在哪个环境中创建。



```javascript
if (Array.isArray(value)){
   //对数组执行某些操作
}
```



Array.isArray() 本质上检测的是对象的 [[Class]] 值，[[Class]] 是对象的一个内部属性，里面包含了对象的类型信息，其格式为 [object Xxx] ，Xxx 就是对应的具体类型 。对于数组而言，[[Class]] 的值就是 [object Array] 。



#### constructor.


当一个函数 F被定义时，JS引擎会为F添加 prototype 原型，然后再在 prototype上添加一个 constructor 属性，并让其指向 F 的引用。如下所示：



![1574647154446-fcf27cea-088e-4303-9d71-0427294f4616.png](/assets/img/posts/Web前端/JavaScript/1574647154446-fcf27cea-088e-4303-9d71-0427294f4616-600262.png)



当执行 var f = new F() 时，F 被当成了构造函数，f 是F的实例对象，此时 F 原型上的 constructor 传递到了 f 上，因此 f.constructor == F



![1574647159208-47f6f525-f09b-4563-bde2-4633749905ca.png](/assets/img/posts/Web前端/JavaScript/1574647159208-47f6f525-f09b-4563-bde2-4633749905ca-763527.png)



以看出，F 利用原型对象上的 constructor 引用了自身，当 F 作为构造函数来创建对象时，原型上的 constructor 就被遗传到了新创建的对象上， 从原型链角度讲，构造函数 F 就是新对象的类型。这样做的意义是，让新对象在诞生以后，就具有可追溯的数据类型。



同样，JavaScript 中的内置对象在内部构建时也是这样做的：



![1574647163755-a7ffa887-726b-4254-a890-ff77d6c33a61.png](/assets/img/posts/Web前端/JavaScript/1574647163755-a7ffa887-726b-4254-a890-ff77d6c33a61-130208.png)



**细节问题：**



> 1. null 和 undefined 是无效的对象，因此是不会有 constructor 存在的，这两种类型的数据需要通过其他方式来判断。
> 2. 函数的 constructor 是不稳定的，这个主要体现在自定义对象上，当开发者重写 prototype 后，原有的 constructor 引用会丢失，constructor 会默认为 Object2. 函数的 constructor 是不稳定的，这个主要体现在自定义对象上，当开发者重写 prototype 后，原有的 constructor 引用会丢失，constructor 会默认为 Object



![1574647182735-114e1b07-da1f-4f28-9c59-6124166610fc.png](/assets/img/posts/Web前端/JavaScript/1574647182735-114e1b07-da1f-4f28-9c59-6124166610fc-016503.png)



为什么变成了 Object？



因为 prototype 被重新赋值的是一个 { }， { } 是 new Object() 的字面量，因此 new Object() 会将 Object 原型上的 constructor 传递给 { }，也就是 Object 本身。



因此，为了规范开发，在重写对象原型时一般都需要重新给 constructor 赋值，以保证对象实例的类型不被篡改。



#### toString


toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。



对于 Object 对象，直接调用 toString()  就能返回 [object Object] 。而对于其他对象，则需要通过 call / apply 来调用才能返回正确的类型信息。



```javascript
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(Symbol()); //[object Symbol]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(window) ; //[object global] window 是全局对象 global 的引用
```



### 扩展 - Symbol


ES5对象属性名都是字符串容易造成属性名的冲突。



```javascript
var a = { name: 'lucy'};
a.name = 'lili';
//这样就会重写属性
```



ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。



注意，Symbol函数前不能使用new命令，否则会报错。这是因为生成的Symbol是一个原始类型的值，不是对象



Symbol函数可以接受一个字符串作为参数，表示对Symbol实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。



```javascript
// 没有参数的情况
var s1 = Symbol();
var s2 = Symbol();
s1 === s2 // false

// 有参数的情况
var s1 = Symbol("foo");
var s2 = Symbol("foo");
s1 === s2 // false
```



Symbol值不能与其他类型的值进行运算



**作为属性名的Symbol**



```javascript
var mySymbol = Symbol();

// 第一种写法
var a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
var a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
var a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```



注意，Symbol值作为对象属性名时，不能用点运算符。



```javascript
var a = {};
var name = Symbol();
a.name = 'lili';
a[name] = 'lucy';
console.log(a.name,a[name]);//lili,lucy
```



Symbol值作为属性名时，该属性还是公开属性，不是私有属性。



这个有点类似于java中的protected属性（protected和private的区别：在类的外部都是不可以访问的，在类内的子类可以继承protected不可以继承private）



但是这里的Symbol在类外部也是可以访问的，只是不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()返回。但有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有Symbol属性名



**Symbol.for()，Symbol.keyFor()**



Symbol.for机制有点类似于单例模式，首先在全局中搜索有没有以该参数作为名称的Symbol值，如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值。和直接的Symbol就点不同了。



```javascript
var s1 = Symbol.for('foo');
var s2 = Symbol.for('foo');
s1 === s2 // true
```



Symbol.keyFor方法返回一个已登记的Symbol类型值的key。实质就是检测该Symbol是否已创建



```javascript
var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

var s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```



**内置的Symbol值**



> 有11个方法，具体可以查看 [https://es6.ruanyifeng.com/#docs/symbol](https://es6.ruanyifeng.com/#docs/symbol)



感谢阅读，本文整理自：  
https://www.cnblogs.com/onepixel/p/5126046.html；  
[https://www.cnblogs.com/sker/p/5474591.html](https://www.cnblogs.com/sker/p/5474591.html)



