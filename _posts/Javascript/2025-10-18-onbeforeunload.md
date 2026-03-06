---
title: onbeforeunload
date: 2025-10-18
description: onbeforeunload
tags: [Javascript]
categories: [Javascript]
---
# onbeforeunload



<font style="color:#DF402A;">浏览器自身机制问题，必须用户点击或者有操作该页面，才会触发该事件，否则无效！</font>



/* 捕获浏览器关闭start */



**window.onbeforeunload = function (event) {**

**       return "是否关闭？";**

**}**



body标签上加上事件，当用户关闭浏览器执行该事件，处理一些事务！



** onunload="removeKey()" onbeforeunload="removeKey()"**



> 更新: 2019-11-28 10:00:26  
> 原文: <https://www.yuque.com/hutaoao/blog/ncceug>