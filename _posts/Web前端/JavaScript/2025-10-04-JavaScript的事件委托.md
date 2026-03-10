---
title: JavaScript的事件委托
date: 2025-10-04
description: JavaScript的事件委托
tags: [Javascript, javascript]
categories: [Web前端, JavaScript]
---
# JavaScript的事件委托

什么是事件委托



把目标元素的事件委托给父元素，利用了[**事件冒泡**](https://hutaoao.github.io/JavascriptEvent/)的原理



### 为什么要使用事件委托


+ 绑定事件太多，浏览器占用内存变大，严重影响性能
+ Ajax出现，局部刷新盛行，每次加载完，都要重新绑定事件
+ 部分浏览器移除元素时，绑定的事件没有被及时移除，导致内存泄漏，严重影响性能
+ Ajax中重复绑定，导致代码耦合性过大，影响后期维护



### 事件委托的好处


+ 事件委托技术可以避免对每个字元素添加事件监听器，减少操作DOM节点的次数，从而减少浏览器的重绘和重排，提高代码的性能。
+ 使用事件委托，只有父元素与DOM存在交互，其他的操作都是在JS虚拟内存中完成的，这样就大大提高了性能。



### 事件委托的应用场景


获取一个列表ul下面的很多li标签里面内容，首先想到的办法就是给每个li标签绑定事件，这样过于繁琐。现在我们利用事件委托，给父元素ul标签绑定事件，通过事件代理去找到要点击的li，获取其内容。



### 事件委托三部曲：


第一步：给父元素绑定事件  
给元素ul添加绑定事件，通过addEventListener为点击事件click添加绑定



第二步：监听子元素的冒泡事件  
这里默认是冒泡，点击子元素li会向上冒泡



第三步：找到是哪个子元素的事件  
通过匿名回调函数的参数e用来接收事件对象，通过target获取触发事件的目标



### 案例


使用原生JS实现事件委托



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件委托</title>
    <style>
        ul{list-style: none;}
        li{border: 1px solid #eee;width: 100px;line-height: 30px;text-align: center;}
    </style>
</head>
<body>
    <ul>
        <li>我是第一个</li>
        <li>我是第二个</li>
        <li>我是第三个</li>
        <li>我是第四个</li>
        <li>我是第五个</li>
    </ul>

    <script>
        const oUl = document.querySelector('ul');
        oUl.addEventListener('click',function (e) {
            console.log(e.target.innerHTML);
        })
    </script>
</body>
</html>
```





