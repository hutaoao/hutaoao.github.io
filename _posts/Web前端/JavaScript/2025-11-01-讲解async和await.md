---
title: 讲解async和await
date: 2025-11-01
description: 讲解async和await
tags: [javascript]
categories: [Web前端, JavaScript]
---
# 讲解async和await

async和await要搭配Promise使用, 它进一步极大的改进了Promise的写法



来看一个简单的场景:

```javascript
//假设有4个异步方法要按顺序调用
new Promise(function(resolve){
    ajaxA("xxxx", ()=> { resolve(); })    
}).then(function(){
    return new Promise(function(resolve){
        ajaxB("xxxx", ()=> { resolve(); })    
    })
}).then(function(){
    return new Promise(function(resolve){
        ajaxC("xxxx", ()=> { resolve(); })    
    })
}).then(function(){
    ajaxD("xxxx");
});
```



语法上不够简洁, 我们可以稍微改造一下

```javascript
//将请求改造成一个通用函数
function request(options) {
    //.....
    return new Promise(....); //使用Promise执行请求,并返回Promise对象
}
//于是我们就可以来发送请求了
request("http://xxxxxx")
.then((data)=>{
    //处理data
})
```



然后我们再来重新改造开头的代码

```javascript
request("ajaxA")
.then((data)=>{
   //处理data
   return request("ajaxB")
})
.then((data)=>{
   //处理data
   return request("ajaxC")
})
.then((data)=>{
   //处理data
   return request("ajaxD")
})
```



比起之前有了不小的进步, 但是看上去依然不够简洁

**如果我能像使用同步代码那样, 使用Promise就好了**

于是, async \ await出现了

```javascript
async function load(){
    await request("ajaxA");
    await request("ajaxB");
    await request("ajaxC");
    await request("ajaxD");
}
await关键字使用的要求非常简单, 后面调用的函数要返回一个Promise对象
load()这个函数已经不再是普通函数, 它出现了await这样"阻塞式"的操作
因此async关键字在这是不能省略的
```



那么现在看, 这段代码变得异常清秀

代码的编写顺序

代码的阅读顺序

代码的执行顺序

全部都是按照同步的习惯来的

是不是很方便.



到这你已经学会了async和await基本使用方式

下面来简单解释一下它的工作流程

```javascript
//wait这个单词是等待的意思
async function load(){
    await request("ajaxA");  //那么这里就是在等待ajaxA请求的完成
    await request("ajaxB");
    await request("ajaxC");
    await request("ajaxD");
}
```



如果后一个请求需要前一个请求的结果怎么办呢?

传统的写法是这样的

```javascript
request("ajaxA")
.then((data1)=>{
   return request("ajaxB", data1);
})
.then((data2)=>{
   return request("ajaxC", data2)
})
.then((data3)=>{
   return request("ajaxD", data3)
})
```



而使用async/await是这样的

```javascript
async function load(){
    let data1 = await request("ajaxA");  
    let data2 = await request("ajaxB", data1);
    let data3 = await request("ajaxC", data2);
    let data4 = await request("ajaxD", data3);
    //await不仅等待Promise完成, 而且还拿到了resolve方法的参数
}
```



注意当一个函数被async修饰以后, 它的返回值会被自动处理成Promise对象

关于异常处理

```javascript
async function load(){
    //请求失败后的处理, 可以使用try-catch来进行
    try{
        let data1 = await request("ajaxA");  
        let data2 = await request("ajaxB", data1);
        let data3 = await request("ajaxC", data2);
    } catch(e){
        //......
    }
}
```



转自：[https://zhuanlan.zhihu.com/p/112081953](https://zhuanlan.zhihu.com/p/112081953)



