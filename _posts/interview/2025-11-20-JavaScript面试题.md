---
layout: post
title: "JavaScript 面试题"
description: "JavaScript 核心面试题汇总,涵盖数据类型、作用域、闭包、原型链、异步编程、ES6 新特性等重要知识点"
date: 2025-11-20 10:00:00 +0800
categories: [面试题, JavaScript]
tags: [javascript, es6, 前端面试, 闭包, 原型链, 异步]
---

## JavaScript 数据类型
**基本类型(原始类型)7 种**:String、Number、Boolean、Null、Undefined、Symbol(ES6 引入)、BigInt(ES2020 引入)

**引用数据类型(对象类型)**:对象、数组、函数

**其它应用类型:** Date、RegExp、Map、Set 等

> `Symbol` 是 ES6 引入了一种新的原始数据类型,表示独一无二的值。
>
> 应用场景:
>
> 1. **创建唯一的对象属性**:定义特定用途的内部属性或库的私有属性时特别有用
> 2. **作为对象的私有属性**:Symbol 属性不会被常规的枚举方法如 for...in 循环或 Object.keys()所包含
>
> `BigInt` 进行**任意精度的整数算术**:可以用来表示或操作任意大的整数,只受限于系统的可用内存
>
> 注:Function、Array 都继承于 Object

**存储区别**

基本数据类型和引用数据类型存储在内存中的位置不同:

* **基本数据类型**存储在**栈**中
* **引用类型**的对象存储于**堆**中

> 主要在于它们在**内存中的存储方式**以及**比较方式**。基本类型直接**存储值本身**,而引用类型存储的是指向实际数据的**内存地址的引用(堆)**。同时,比较基本类型值时,比较的是它们的**值**,而比较应用类型值时,比较的是它们的**引用**。

## Javascript 数据类型判断
### 1. typeof - 返回变量的基本类型
> 弊端:判断 **null** 和 **引用数据类型** 时返回 object(function 除外)

```javascript
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
typeof null // 'object'
typeof [] // 'object'
typeof {} // 'object'
typeof console // 'object'
typeof console.log // 'function'
```

### 2. instanceof - 返回布尔值
**用于判断引用类型的继承关系**

> 弊端:不支持基本数据类型判断

```javascript
Function instanceof Object  //true
Date instanceof Object  //true
//不支持基本数据类型判断
"123" instanceof String  //false
123 instanceof Number  //false
```

> `Object.setPrototypeOf` 能改对象的原型

```javascript
const obj = {};
console.log(obj instanceof Object); // true
Object.setPrototypeOf(obj, Array.prototype);
console.log(obj instanceof Array); // true
```

### 3. Object.prototype.toString - **最佳(ES6 之前)** - **可以精确的判断各种数据类型**
统一返回格式 "[object Xxx]" 的字符串

```javascript
Object.prototype.toString({})       // "[object Object]"
Object.prototype.toString.call({})  // "[object Object]"
Object.prototype.toString.call(1)    // "[object Number]"
Object.prototype.toString.call('1')  // "[object String]"
Object.prototype.toString.call(true)  // "[object Boolean]"
Object.prototype.toString.call(function(){})  // "[object Function]"
Object.prototype.toString.call(null)   //"[object Null]"
Object.prototype.toString.call(undefined) //"[object Undefined]"
Object.prototype.toString.call(/123/g)    //"[object RegExp]"
Object.prototype.toString.call(new Date()) //"[object Date]"
Object.prototype.toString.call([])       //"[object Array]"
Object.prototype.toString.call(document)  //"[object HTMLDocument]"
Object.prototype.toString.call(window)   //"[object Window]"
```

> ES6 可以通过 `Symbol.toStringTag` 进行修改对应的 type 值

### 4. **⚠️判断数组最佳方案 Array.isArray()  👍 👍 👍 👍 👍**

## JSON 的了解
`JSON(JavaScript Object Notation)` 是一种轻量级的数据交换格式。

它是基于 JavaScript 的一个子集。数据格式简单, 易于读写, 占用带宽小

## javascript 的 typeof 返回哪些数据类型
string、boolean、number、undefined、function、object、symbol

## 类型转换
* **强制转换**(显示转换)
* **自动转换**(隐式转换)

强制:parseInt()、parseFloat()、Number()、String()、Boolean()

隐式:==、 ===、+、-、*、/、%

> 1=="1"      //true
> null==undefined      //true

## 如何阻止事件冒泡
IE:ev.cancelBubble = true;非 IE:ev.stopPropagation();

## js 延迟加载的方式有哪些
**延迟加载**是一种优化技术,用于延迟加载某些资源(如图像、视频、样式表等),以减少页面加载时间并提高性能。

延迟加载通常用于**在用户需要时才加载相关资源**,而不是在页面加载时一次性加载所有资源。

* `defer` 用于`<script>`标签的属性,它指示浏览器延迟脚本的加载和执行,直到整个 HTML 文档都解析完成
* `async` 异步脚本,用于`<script>`标签的属性,它指示浏览器异步加载和执行脚本:使用`async`属性的脚本会**立即开始加载**
* **动态创建 DOM 方式**:动态创建`script`标签,当页面的全部内容加载完毕后,在执行创建挂载,加载完毕后 callBack)
* **按需加载**:用于大型应用,可以将应用分割成多个模块,按需加载。可以使用 webpack 等打包工具进行按需打包
* **延迟加载**:将某些代码包裹在条件语句中,当满足条件时才执行

## null,undefined 的区别
`null` **表示一个尚未存在的对象**,转换为数值时为 0;

`undefined` **表示声明的变量未初始化**,转换为数值时为 NAN;

typeof(null) -- object;

typeof(undefined) -- undefined

## 说说 JavaScript 的基本规范
1) 不要在同一行声明多个变量

2) 使用 === 或 !== 来比较 true/false 或者数值

3) switch 必须带有 default 分支

4) 函数应该有返回值

5) for if else 必须使用大括号

6) 语句结束加分号

7) 命名要有意义,使用驼峰命名法

## 什么是闭包
**闭包**:**能够读取其它函数内部变量的函数**。

又或者:函数 A 里面包含了 函数 B,而 函数 B 里面使用了 函数 A 的变量,那么 函数 B 被称为闭包。

优点:避免全局变量污染。缺点:常驻内存,容易造成内存泄漏。

说明:

> 闭包的定义是能够读取其它函数内部变量的函数,其实就是将函数内部和函数外部连接的一座桥梁
> 闭包另一个作用就是**避免全局变量的污染**,**让这些变量的值始终保持在内存中**,同时副作用就是容易造成**内存泄漏**

### 闭包的经典问题

```javascript
for(var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}

// 输出: 3 3 3
// 首先,for 循环是同步代码,先执行三遍 for,i 变成了 3
// 然后,再执行异步代码 setTimeout,这时候输出的 i,只能是 3 个 3 了
```

### 解决方案
#### 1) 使用 let

```javascript
for(let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}

// 在这里,每个 let 和代码块结合起来形成块级作用域
// 当 setTimeout() 打印时,会寻找最近的块级作用域中的 i,所以依次打印出 0 1 2
```

#### 2) 使用立即执行函数解决闭包的问题

```javascript
for(var i = 0; i < 3; i++) {
  (function(i){
    setTimeout(function() {
      console.log(i);
    }, 1000);
  })(i)
}
```

> **!!! 立即执行函数:** 创建一个独立的作用域。这个作用域里面的变量,外面访问不到(即避免了「变量污染」)

## JS 作用域及作用域链
### 作用域
在 JavaScript 中,作用域分为 **全局作用域** 和 **函数作用域**

* **全局作用域:** 代码在程序的任何地方都能被访问,**window 对象的内置属性都拥有全局作用域**
  * 在最外层函数外面定义的变量拥有全局作用域
  * 所有末定义直接赋值的变量自动声明为拥有全局作用域(不论函数内外)
* **函数作用域:** 在固定的代码片段才能被访问
* **块级作用域:** **let/const**,在大括号中使用 let 和 const 声明的变量存在于块级作用域中。在大括号之外不能访问这些变量

作用域有上下级关系,上下级关系的确定就看函数是在哪个作用域下创建的。

如上:`fn` 作用域下创建了`bar`函数,那么"fn 作用域"就是"bar 作用域"的上级。

作用域最大的用处就是**隔离变量**,不同作用域下同名变量不会有冲突。

### 作用域链
当使用一个变量的时候:首先 js 引擎会在当前作用域下去寻找,如果没找到 就会向上级作用域去查,直到找到该变量或查到全局作用域,这么一个查找过程形成的链条就叫做**作用域链**

## let 、var、const 的区别
|  | **const** | **let** | **var** |
| --- | --- | :---: | :---: |
| 全局污染 | 没有 | | 声明的变量挂到 window 了 |
| 暂存性死区 | 存在暂存性死区 | | 不存在 |
| 块级作用域 | 块级作用域 | | 没有块级作用域 |
| 变量提升 | 不支持变量提升 | | 支持变量提升 |
| 重复声明 | 不允许重复声明同名变量 | | 可以重复声明变量,后声明的同名变量会覆盖之前声明的变量 |
| 声明赋值 | 声明同时必须赋值 | 可以先声明再赋值 | |
| 修改 | 声明的是常量不可以修改 | 声明赋值后变量值还可以修改 | |

> 浏览器的全局对象是 **window**,Node 的全局对象是 **global**
>
> **暂时性死区(TDZ):** 在使用 let、const 命令声明变量之前,该变量都是不可用的(报错),var 可以使用 打印出来 undefined
>
> **变量提升**:即变量可以在声明之前使用
>
> **块级作用域**:指通过大括号({})包裹的代码块的作用域

```javascript
let x
x = 1
console.log(x) // 1

const x
x = 1
console.log(x) // Uncaught SyntaxError: Missing initializer in const declaration

function fun() {
  let a = 1;
  if (a === 1) {
    var b = 2;
    let c = 3;

    for(var i = 0; i < 5; i++){
      console.log(i); // 0 1 2 3 4
    }
  }
  console.log(i) // 5
  console.log(b) // 2
  console.log(c) // c is not defined
}
fun()
```

## 原型、原型链
JS 每个**函数**都有 `prototype` 属性 称之为原型,因为这个属性的值是个对象,也称之为 **原型对象:**用来存放一些属性和方法,共享给实例对象使用;

所有实例对象都有一个 **`__proto__`** 属性,这个属性指向它的原型对象,所以**实例对象都会继承这个原型对象的属性和方法**;

原型对象上也有一个 **`__proto__`** 属性 指向 Object 原型对象,而 Object 原型对象的 **`__proto__`** 指向的是 null。

**当我们访问对象的某个属性时,就会从实例对象,原型对象,Object 原型对象上层层寻找,形成的链式结构称为** **原型链** **。

```javascript
function Person() {
    console.log(1)
}
Person.prototype.name = 'bob'
let person = new Person() // 1

// person.__proto__ 指向 Person.prototype
console.log(person.__proto__, person.__proto__ === Person.prototype) // {name: "bob", [[Prototype]]: Object} true

// person.__proto__.__proto__ 指向 Object.prototype
console.log(person.__proto__.__proto__) // {}

// 下面 Object.prototype 指向了 null
console.log(person.__proto__.__proto__.__proto__) // null
```

> 原型的作用:JS 语言要实现面向对象,避免了类型的丢失。

## GET 和 POST 区别
* **协议层面:** 语意区别,GET 表示获取数据 POST 表示提交一些东西
* **应用层面:** GET 请求体为空(不是没有请求体)
* **浏览器层面:**

| GET | POST |
| --- | --- |
| **参数 URL 可见**<br/>+ 参数保留在浏览器历史中<br/>+ 能被缓存<br/>+ 相对安全性较差 | 参数 URL 不可见,**存放在 HTTP 的包体内**<br/>+ **参数不会保存在浏览器历史中**<br/>+ **不能缓存**<br/>+ 相对安全性较好 |
| URL 长度有限制(浏览器限制) | 无限制 |
| 只能发送 ASCII 字符 | 没有限制。也允许二进制数据 |
| **只 URL 编码** | **支持多种编码方式** |

get 和 post 请求缓存补充:

> + get 请求类似于查找的过程,用户获取数据,可以不用每次都与数据库连接,所以**可以使用缓存**。
> + post 不同,post 做的一般是修改和删除的工作,所以必须与数据库交互,所以**不能使用缓存**。因此 get 请求适合于请求缓存。

## get 请求传参长度的误区
误区:我们经常说 get 请求参数的大小存在限制,而 post 请求的参数大小是无限制的

**实际上 HTTP 协议从未规定 GET/POST 的请求长度限制是多少**。对 get 请求参数的**限制是来源于浏览器或 web 服务器**,浏览器或 web 服务器限制了 url 的长度。为了明确这个概念,我们必须再次强调下面几点:

1. HTTP 协议 未规定 GET 和 POST 的长度限制
2. GET 的最大长度显示是因为 浏览器和 web 服务器限制了 URI 的长度
3. 不同的浏览器和 WEB 服务器,限制的最大长度不一样
4. 要支持 IE,则最大长度为 2083byte,若只支持 Chrome,则最大长度 8182byte

## JS 用递归的方式写 1 到 100 求和 - 闭包
```javascript
function sumToN(n) {
  if (n === 1) {
    return 1;
  } else {
    return n + sumToN(n - 1);
  }
}

console.log(sumToN(100)); // 输出 5050
```

## target、currentTarget 的区别
`target` 和 `currentTarget` 都是事件对象中的属性

* **target** 属性代表触发事件的 DOM 元素;在事件流的目标阶段;**不会随着事件在 DOM 树中的传递而改变**
* **currentTarget** 属性表示当前正在处理事件的元素,这可能是事件的源头,也可能不是;在事件流的捕获、目标及冒泡阶段;**可能会随着事件在 DOM 树中的传递而改变**

> 如果你只关心事件最初发生的元素,可以使用 target
>
> 如果你需要跟踪事件在整个 DOM 树中的传递过程,可以使用 currentTarget

## export 和 export default 的区别
1. **数量不同**:export 可以导出多个;export default 只能默认的,只能有一个
2. **使用时的语法不同**:export default 导入导出时不用加 `{}`;export 导入导出一般 要加 `{}`。

```javascript
export default  xxx
import xxx from './'

export xxx
import {xxx} from './'
```

## 什么是会话 cookie,什么是持久 cookie
**会话 cookie:** 只在当前浏览器会话期间有效,当用户关闭浏览器时,会话 cookie 会被自动删除。主要用于在用户访问网站时保存用户的登录状态,以及其他与当前会话相关的信息。

**持久 cookie:** 在浏览器关闭后仍然存在,并在**指定的过期日期**之前一直有效。它们通常用于跟踪用户的偏好设置、记录用户的登录状态,以及实现其他**需要长时间保存**的功能。持久 cookie 可以设置一个过期日期,当超过该日期时,cookie 将自动过期并被删除。

> `cookie` 如果没有指定有效期,那么这个 Cookie 就是 Session Cookie,只在当前会话有效,如果关闭浏览器,那么 Cookie 就会被删除

## 浏览器缓存机制
指的是通过在一段时间内保留已接收到的 web 资源的一个副本。如果发起了对这个资源的再一次请求,那么浏览器会直接使用缓存的副本,而不是向服务器发起请求。

其机制是**根据 HTTP 报文的缓存标识(头部信息)** 来控制缓存行为。

## documen.write 和 innerHTML 的区别
* document.write 只能重绘整个页面
* innerHTML 可以重绘页面的一部分

## 什么叫优雅降级和渐进增强
优雅降级和渐进增强是两种**前端开发策略**,它们都**旨在提高网站或应用的可用性和可访问性**

* **优雅降级:** 它要求首先构建一个完整、功能齐全的网站或应用,然后逐步移除或降低某些功能,直到只留下最基本的、满足所有用户需求的功能。

> 针对低版本浏览器进行构建页面,完成基本的功能。

* **渐进增强:** 它要求首先构建一个基本的、能够满足所有用户需求的网站或应用,然后逐步添加更多的特性和功能,以提供更好的用户体验

> 针对最高级、最完善的浏览器进行构建页面,追求完整功能。

## JS 如何实现继承
1. 原型链继承
2. 借用构造函数继承
3. 组合继承(原型+借用构造)
4. 原型式继承
5. 寄生式继承
6. 寄生组合式继承

## Ajax 原理、过程及如何中断
**核心:**

1. Ajax 就是**异步的 JS 和 XML**(目前我们一般用 JSON 代替 XML)
2. Ajax 主要用于**不刷新页面**的情况下浏览器发起请求并接受响应,**局部更新**
3. Ajax 核心点是 **XMLHttpRequest** 对象

**缺点:**

1. 不能进行跨域访问(解决:**JSONP、CORS**)

**过程:**

1. 创建 XMLHttpRequest 对象
2. 连接服务器(使用 open 方法),并指定请求方式、URL 等
3. 发送 HTTP 请求(使用 send 方法)
4. 获取返回数据(xhr.onreadystatechange),根据状态码(xhr.readyState)判断。

**如何中断:**

1. 设置超时时间让 ajax 自动断开
2. 手动停止 ajax 请求

> 其核心是调用 XML 对象的 abort 方法,**ajax.abort()**

## 同源策略及跨域解决方案
【协议+域名+端口号】相同,即为同源,只能向同源的服务发起 AJAX 请求。

是浏览器的基本安全策略,否则会很容易受到 XSS、CSRF 攻击。

**实现跨域请求:**

* **JSONP:** 借助于`<script>`标签元素,因为`<script>`的 `src` 可以访问任何站点的资源,需要服务端对应接口支持,所以是需要双方约定好;仅支持 GET 方式请求,通过回调函数来接收数据
* **CORS:** 原理就是在请求头加入一个身份来源标识,服务端根据这个标识来判等是否允许访问
* **WebSocket:** WebSocket 可以实现浏览器与服务端的双向通信,允许跨域通讯
* **iframe+postMessage:** 使用 window.postMessage()来实现窗口之间的通信
* **nginx 反向代理:** 生产环境常用做法
* **node 服务代理:** 本地开发常用做法

## CORS(跨域资源共享)
**CORS** 是一种新标准,**支持同源通信,也支持跨域通信**,fetch 是实现 CORS 通信的。

CORS 需要浏览器和服务器同时支持,实现 CORS 通信的关键是服务器。只要服务器实现了 CORS 接口(CORS 通过在服务器端设置特殊的 HTTP 头部信息来实现跨域资源共享),就可以跨源通信。

### CORS 简单请求和预检请求的区别如下:

#### 1. 简单请求:
请求方式:[GET](https://so.csdn.net/so/search?q=GET&spm=1001.2101.3001.7020)、POST、HEAD 三者之一

请求头(仅包含安全的字段,常见的安全字段如下):

- Accept
- Accept-Language
- Content-Language
- DPR
- Downlink
- Save-Data
- Viewport-Width
- Width
- Content-Type

Content-Type:只有三个值 text/plain、multipart/form-data、application/x-www-form-urlencoded

#### 2. 预检请求:
只要符合以下任何一个条件的请求,都需要进行预检请求:

- 请求方式为 GET、POST、HEAD 之外的请求 Method 类型
- 请求头中包含自定义头部字段
- 向服务器发送了 application/json 格式的数据

+ **请求方式**:简单请求只发起一次网络请求;预检请求会发起两次请求,先发起一个 option 请求,获得服务器响应后才会发起真正的请求
+ **请求头信息**:简单请求是指请求方法是 HEAD、GET、POST 之一,并且 Content-Type 的值只限于 text/plain、multipart/form-data、application/x-www-form-urlencoded,同时请求头中只包含 Accept 等字段;预检请求则没有这些限制。

## 同步和异步任务
JS 特点是单线程(同一时间只能做同一件事)

**同步:** 前一个任务执行完成后再执行下一个任务(**一次只能做一个任务**)

**异步:** 执行一个任务的同时还可以去执行下一个任务(**可以同时做多个任务**)

> setTimeout、setInterval、Ajax 异步请求、ES6 的 Promise

## JS 是单线程的,为什么 JS 能有异步任务?
`JavaScript` 是一门**单线程**的语言,意味着_同一时间内只能做一件事_,但它可以通过**事件循环**机制来处理异步任务。

**事件循环**允许 JS 在等待异步操作(如网络请求或定时器)完成时执行其他任务,并在异步操作完成后通过回调函数或 Promise 来处理结果。

事件循环中,JavaScript 会按照顺序执行任务队列中的任务,直到队列为空。当遇到异步操作时,JavaScript 会将异步操作放入任务队列中,并继续执行其他任务。当异步操作完成后,JavaScript 会从任务队列中取出该任务,并执行相关的回调函数或 Promise 的 then 方法。

## 防抖和节流的区别
**防抖:** 在单位时间内**频繁触发事件**,**只有最后一次生效**

**理解:** 即防止抖动。抖动时就先不管它,等啥时候静止了,再做操作

比如:在游戏回城的时候被打断,再次点回城就会重新计时,最终只有没被打断的最后一次,才能成功回城,就是防抖

**应用场景:** (1)手机号、邮箱地址的校验 (2)当用户 input 框输入完成后再发请求,如搜索等

**节流:** 在单位时间内频繁触发事件,只生效一次(也就是**只有第一次生效**)

理解:即节省交互沟通。流,可理解为交流,不一定会产生网络流量。

比如:鼠标点击下一张轮播图,不管单位时间内连续点击了多少次,轮播图都是 2 秒换下一张,就是节流

**应用场景:** 高频事件,如:多少秒之后获取验证码、resize 事件和 scroll 事件等

## 数组常用方法
* **map**:遍历数组,返回 回调返回值 组成的新数组
* **forEach**:无法 break,可以用 try/catch 中 throw new Error 来停止
* **filter**:过滤,返回符合条件的新数组
* **some**:有一个条件返回 true,则整体为 true
* **every**:有一个条件返回 false,则整体为 false,即所有条件返回 true 才会返回 true
* **find:**返回第一个匹配的元素
* **join**:通过指定连接符生成字符串
* **push / unshift**:头部添加/末尾删除,改变原数组,**返回数组的最新长度**
* **pop** **/ shift**:末尾删除/头部删除,改变原数组,**返回被删除的项**
* **sort(fn) / reverse:**排序与反转,改变原数组
* **concat**:连接数组,不影响原数组, 浅拷贝
* **slice(start, end)**:返回截断后的新数组,不改变原数组
* **splice(start, number, value...)**:删除/添加元素,value 为插入项,改变原数组,返回删除元素组成的数组
* **indexOf / lastIndexOf(value, fromIndex):**查找数组项,返回对应的下标
* **reduce / reduceRight(fn(prev, cur), defaultPrev):**两两执行,prev 为上次化简函数的 return 值,cur 为当前值(从第二项开始)
* **includes**:返回要查找的元素在数组中的位置,找到返回 true,否则 false

## 数组去重
### 1. 使用 `Set`

使用了`Set`对象的特性,它**只允许包含唯一值的集合**,通过将数组转化为 Set,然后再将 Set 转回为数组,即可得到去重后的数组。

```javascript
const array = [1, 2, 3, 4, 4, 5, 6, 6];
// Array.from方法可以将 Set 结构转为数组。
const uniqueArray = Array.from(new Set(array));
// 或
const uniqueArray = [...new Set(array)];
console.log(uniqueArray);  // [1, 2, 3, 4, 5, 6]
```

### 2. 使用 `reduce` 方法

利用 reduce 方法迭代数组,将不重复的值添加到一个累加器数组中,然后利用 reduce。

```javascript
const array = [1, 2, 3, 4, 4, 5, 6, 6];
const uniqueArray = array.reduce((acc, cur) => {
  if (!acc.includes(cur)) {
    acc.push(cur);
  }
  return acc;
}, []);
console.log(uniqueArray);  // [1, 2, 3, 4, 5, 6]
```

### 3. 使用`filter` 方法和 `indexOf`

使用 filter 方法和 indexOf 来检查每个元素是否在数组中的**第一个出现位置**,从而筛选出不重复的元素。

```javascript
const array = [1, 2, 3, 4, 4, 5, 6, 6];
const uniqueArray = array.filter((value, index, self) => {
  return self.indexOf(value) === index;
});
console.log(uniqueArray);  // [1, 2, 3, 4, 5, 6]
```

### 4. 使用 ES6 的 `Map`

使用 ES6 的 Map 对象,通过将数组元素映射成 [key, value] 的形式,并将其转化为 Map 对象后,再通过扩展操作符将 Map 对象转回为数组。

```javascript
const array = [1, 2, 3, 4, 4, 5, 6, 6];
const uniqueArray = [...new Map(array.map(item => [item, item])).values()];
console.log(uniqueArray);  // [1, 2, 3, 4, 5, 6]
```

## ES6 的 Set 和 Map 区别
* **性质不同**:Set 是值的**集合**,Map 是**键值对**
* **存储方式不同**:Set 以 [value, value] 的形式储存元素,Map 以 [key, value] (键值对) 的形式储存
* **获取方式不同**:Set 通过 [] 获取,Map 通过 **get()** 获取
* **用途不同**:Set 主要应用在数组去重;Map 由于没有格式限制,Map 主要应用在数据存储

## ES6 新特性
`ES6` 是 JavaScript 语言的下一代标准,它的目标是使得 JavaScript 语言可以用来**编写复杂的大型应用程序**

* **块级作用域**,可以在块级作用域中声明变量
* **箭头函数**,一种新的函数声明方式
* **解构赋值**,一种从数组或对象中提取值并赋值给变量的方式
* **默认参数**,允许在函数定义时为参数提供默认值
* **扩展运算符**,可以将数组或对象展开,提取出其中的元素
* **模板字符串**
* **类和模块**
* **迭代器和生成器**
* **Promise 对象**
* **Set 和 Map 数据结构**
* **模块化导入和导出**等等

## 箭头函数和普通函数的区别
1. **外形不同**:箭头函数使用箭头定义,普通函数中没有
2. **箭头函数全都是匿名函数**:普通函数可以有匿名函数,也可以有具名函数
3. **箭头函数不能用于构造函数**:普通函数可以用于构造函数,以此创建对象实例
4. **箭头函数中 this 的指向不同**:在普通函数中,this 总是指向调用它的对象,如果用作构造函数,它指向创建的对象实例
5. **箭头函数不具有 arguments 对象**:每一个普通函数调用后都具有一个 arguments 对象,用来存储实际传递的参数。但是箭头函数并没有此对象
6. **其他区别**:箭头函数不具有 prototype 原型对象;箭头函数不具有 super;箭头函数不具有 new.target

## 为什么需要箭头函数
**消除函数二义性** - 二义性(两个层面的含义)

### 1. 指令序列

```javascript
function fun() {}

fun()
```

### 2. 创建实例

```javascript
function Fun() {}

new Fun()
```

上面就是函数的两层意思:在过去的 JS 中没有做区分(设计缺陷),看到一个函数时 不知如何调用 -> 二义性

> ES6 中的重要理念:消除函数二义性,引入了 **class** 和 **箭头函数**
>
> `class` 不能使用直接调用,而尖头函数也不能用 `new` 来调用
>
> 箭头函数(无 this 本质)代表的是指令序列,它跟实例无关,跟面向对象无关,this 来源于面向对象的概念

## 0.1 + 0.2 等于 0.3 吗?为什么?如何解决?
**不等于**

**为什么**:因为 JS 中的浮点数运算存在精度问题:JS 中的浮点数实际上是**以二进制的形式存储的**,而有些数在二进制中并不精确。当这些不精确的数被加在一起时,结果可能会变得更加不精确。

**解决**:可以使用一些第三方库,例如 decimal.js 或者 bignumber.js;或者全部转化整数在计算。

## JS 中哪些方法会改变原数组
* **push()**
* **pop()**
* **shift()**
* **unshift()**
* **splice()**
* **sort()**

## 介绍一下 promise,和 async、await 之间的区别
**Promise 是** ****异步编程的** ****一种解决方案,ES6 新标准(统一 JS 的异步实现方案)**

> 在 promise 之前,异步的实现都是通过回调进行的,回调每个人实现不一样,所以 promise 核心是统一 JS 的异步实现方案,才有 `<async>` `<await>`

有三种状态:pending(进行中)、fulfilled(已成功)和 rejected(已失败),一旦状态变为已成功或已失败,就不可再改变。

使用 `.then()` 方法处理异步操作成功的情况,使用 `.catch()` 方法处理异步操作失败的情况

**处理以下场景问题:**

1. 链式回调 - `<async>` `<await>`
2. 同时发起几个异步请求,谁先有结果就拿谁的 - `Promise.race(requests)`
3. 发起多个请求,等到所有请求后再做下一步处理 - `Promise.all(requests)`

**之间的关系和区别:**

`Promise` 是一种用于**处理异步操作**的对象,通过使用**链式调用**的 then 方法来处理异步操作的结果,而 `async/await` 是基于 `Promise` 的一种**语法糖**,通过使用 `async` 关键字声明一个函数为异步函数,并使用 `await` 关键字来等待 `Promise` 对象的解决,**以同步的方式编写异步操作**的代码。因此 `Promise` 和 `async/await` 是**相辅相成**的关系。
