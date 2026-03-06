---
title: Javascript异步和同步理解-异步编程的4种方法
date: 2025-10-08
description: Javascript异步和同步理解 + 异步编程的4种方法
tags: [Javascript, javascript]
categories: [Javascript]
---
# Javascript异步和同步理解 + 异步编程的4种方法



## 一、Javascript同步和异步介绍


我们知道，javascript语言是一门“**单线程**”的语言，所谓"单线程"，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务，以此类推。



这种模式的好处是实现起来比较简单，执行环境相对单纯；坏处是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。常见的浏览器无响应（假死），往往就是因为某一段Javascript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。



为了解决这个问题，Javascript语言将任务的执行模式分成两种：**同步（Synchronous）**和**异步（Asynchronous）**。



+ "同步模式"：就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；
+ "异步模式"：则完全不同，每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。



"异步模式"非常重要。在浏览器端，耗时很长的操作都应该异步执行，避免浏览器失去响应，最好的例子就是Ajax操作。在服务器端，"异步模式"甚至是唯一的模式，因为执行环境是单线程的，如果允许同步执行所有http请求，服务器性能会急剧下降，很快就会失去响应。



其实同步和异步，无论如何，做事情的时候都是只有一条流水线（单线程），同步和异步的差别就在于这条流水线上各个流程的执行顺序不同。



最基础的异步是**setTimeout**和**setInterval**函数，很常见，但是很少人有人知道其实这就是异步，因为它们可以控制js的执行顺序。我们也可以简单地理解为：可以改变程序正常执行顺序的操作就可以看成是异步操作。如下代码：



```javascript
<script type="text/javascript">
    console.log( "1" );
    setTimeout(function() {
    console.log( "2" )
    }, 0 );
    setTimeout(function() {
    console.log( "3" )
    }, 0 );
    setTimeout(function() {
    console.log( "4" )
    }, 0 );
    console.log( "5" );
</script>
```



输出顺序是什么呢？



![1574645782897-7666c0d5-c402-4304-9f02-fff02b47ed89.png](/assets/img/posts/Javascript/1574645782897-7666c0d5-c402-4304-9f02-fff02b47ed89-983896.png)



可见，尽管我们设置了setTimeout（function，time）中的等待时间为0，结果其中的function还是后执行。



火狐浏览器的api文档有这样一句话：



> Because even though setTimeout was called with a delay of zero, it's placed on a queue and scheduled to run at the next opportunity, not immediately. Currently executing code must complete before functions on the queue are executed, the resulting execution order may not be as expected.
>



意思就是：尽管setTimeout的time延迟时间为0，其中的function也会被放入一个队列中，等待下一个机会执行，当前的代码（指不需要加入队列中的程序）必须在该队列的程序完成之前完成，因此结果可能不与预期结果相同。



这里说到了一个“**队列**”（即**任务队列**），该队列放的是什么呢，放的就是setTimeout中的function，这些function依次加入该队列，即该队列中所有function中的程序将会在该队列以外的所有代码执行完毕之后再以此执行，这是为什么呢？因为在执行程序的时候，浏览器会默认setTimeout以及ajax请求这一类的方法都是耗时程序（尽管可能不耗时），将其加入一个队列中，该队列是一个存储耗时程序的队列，在所有不耗时程序执行过后，再来依次执行该队列中的程序。



又回到了最初的起点——javascript是单线程。单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。于是就有一个概念——任务队列。如果排队是因为计算量大，CPU忙不过来，倒也算了，但是很多时候CPU是闲着的，因为IO设备（输入输出设备）很慢（比如Ajax操作从网络读取数据），不得不等着结果出来，再往下执行。于是JavaScript语言的设计者意识到，这时主线程完全可以不管IO设备，挂起处于等待中的任务，先运行排在后面的任务。等到IO设备返回了结果，再回过头，把挂起的任务继续执行下去。



于是，所有任务可以分成两种，一种是**同步任务（synchronous）**，另一种是**异步任务（asynchronous）**。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有等主线程任务执行完毕，"任务队列"开始通知主线程，请求执行任务，该任务才会进入主线程执行。



具体来说，异步运行机制如下：



1. 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
2. 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
3. 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
4. 主线程不断重复上面的第三步。



**只要主线程空了，就会去读取"任务队列"，这就是JavaScript的运行机制**。这个过程会不断重复。



"任务队列"是一个事件的队列（也可以理解成消息的队列），IO设备完成一项任务，就在"任务队列"中添加一个事件，表示相关的异步任务可以进入"执行栈"了。主线程读取"任务队列"，就是读取里面有哪些事件。  
"任务队列"中的事件，除了IO设备的事件以外，还包括一些用户产生的事件（比如鼠标点击、页面滚动等等），比如$ (selectot).click(function)，这些都是相对耗时的操作。只要指定过这些事件的回调函数，这些事件发生时就会进入"任务队列"，等待主线程读取。
所谓"回调函数"（callback），就是那些会被主线程挂起来的代码，前面说的点击事件 $(selectot).click(function)中的function就是一个回调函数。异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。例如ajax的success，complete，error也都指定了各自的回调函数，这些函数就会加入“任务队列”中，等待执行。



### 二、Javascript异步编程的4种方法


#### 一、回调函数


这是异步编程最基本的方法。



假定有两个函数f1和f2，后者等待前者的执行结果。



```javascript
f1();
f2();
```



如果f1是一个很耗时的任务，可以考虑改写f1，把f2写成f1的回调函数。



```javascript
function f1(callback){
　　setTimeout(function () {
　　　　// f1的任务代码
　　　　callback();
　　}, 1000);
}
```



执行代码就变成下面这样：



```javascript
f1(f2);
```



采用这种方式，我们把同步操作变成了异步操作，f1不会堵塞程序运行，相当于先执行程序的主要逻辑，将耗时的操作推迟执行。



回调函数的优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度**耦合（Coupling）**，流程会很混乱，而且每个任务只能指定一个回调函数。



#### 二、事件监听


另一种思路是采用事件驱动模式。任务的执行不取决于代码的顺序，而取决于某个事件是否发生。



还是以f1和f2为例。首先，为f1绑定一个事件（这里采用的jQuery的写法）。



```javascript
f1.on('done', f2);
```



上面这行代码的意思是，当f1发生done事件，就执行f2。然后，对f1进行改写：



```javascript
function f1(){
　　setTimeout(function () {
　　　　// f1的任务代码
　　　　f1.trigger('done');
　　}, 1000);
}
```



f1.trigger('done')表示，执行完成后，立即触发done事件，从而开始执行f2。



这种方法的优点是比较容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以**"去耦合"（Decoupling）**，有利于实现模块化。缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。



#### 三、发布/订阅


上一节的"事件"，完全可以理解成"信号"。



我们假定，存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"（publish）一个信号，其他任务可以向信号中心**"订阅"（subscribe）**这个信号，从而知道什么时候自己可以开始执行。这就叫做**"发布/订阅模式"（publish-subscribe pattern）**，又称**"观察者模式"（observer pattern）**。



这个模式有多种实现，下面采用的是Ben Alman的Tiny Pub/Sub，这是jQuery的一个插件。



首先，f2向"信号中心"jQuery订阅"done"信号。



```javascript
jQuery.subscribe("done", f2);
```



然后，f1进行如下改写：



```javascript
function f1(){
　　setTimeout(function () {
　　　　// f1的任务代码
　　　　jQuery.publish("done");
　　}, 1000);
}
```



jQuery.publish("done")的意思是，f1执行完成后，向"信号中心"jQuery发布"done"信号，从而引发f2的执行。



此外，f2完成执行后，也可以**取消订阅（unsubscribe）**。



```javascript
jQuery.unsubscribe("done", f2);
```



这种方法的性质与"事件监听"类似，但是明显优于后者。因为我们可以通过查看"消息中心"，了解存在多少信号、每个信号有多少订阅者，从而监控程序的运行。



#### 四、Promises对象


**Promises**对象是**CommonJS工作组**提出的一种规范，目的是为异步编程提供统一接口。



简单说，它的思想是，每一个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数。比如，f1的回调函数f2,可以写成：



```javascript
f1().then(f2);
```



f1要进行如下改写（这里使用的是jQuery的实现）：



```javascript
function f1(){
　　var dfd = $.Deferred();
　　setTimeout(function () {
　　　　// f1的任务代码
　　　　dfd.resolve();
　　}, 500);
　　return dfd.promise;
}
```



这样写的优点在于，回调函数变成了链式写法，程序的流程可以看得很清楚，而且有一整套的配套方法，可以实现许多强大的功能。



比如，指定多个回调函数：



```javascript
f1().then(f2).then(f3);
```



再比如，指定发生错误时的回调函数：



```javascript
f1().then(f2).fail(f3);
```



而且，它还有一个前面三种方法都没有的好处：如果一个任务已经完成，再添加回调函数，该回调函数会立即执行。所以，你不用担心是否错过了某个事件或信号。这种方法的缺点就是编写和理解，都相对比较难。



> 更新: 2026-03-06 11:36:27  
> 原文: <https://www.yuque.com/hutaoao/blog/ntr38g>