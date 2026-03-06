---
title: JavaScript的本地对象-内置对象和宿主对象
date: 2025-10-05
description: JavaScript的本地对象，内置对象和宿主对象
tags: [Javascript, javascript]
categories: [Javascript]
---
# JavaScript的本地对象，内置对象和宿主对象

## 本地对象（native object）也叫原生对象、内部对象


“独立于宿主环境的 ECMAScript 实现提供的对象”。简单来说，本地对象就是 ECMA-262 定义的类（引用类型），**可以 new 实例化的对象**。它们包括：



+ Object
+ Function
+ Array
+ String
+ Boolean
+ Number
+ Date
+ RegExp
+ Error
+ EvalError
+ RangeError
+ ReferenceError
+ SyntaxError
+ TypeError
+ URIError



### 内置对象（Build-in object）


不可以实例化的（他们也是本地对象，**内置对象是本地对象的一个子集**）



在 ECMAScript 程序开始执行时出现，即在引擎初始化阶段就被创建好的对象。这意味着开发者不必明确实例化内置对象，它已被实例化了



如： Gload，Math，arguments，this，event 等等



### 宿主对象（host object）


宿主”就是我们网页的运行环境，即“操作系统”和“浏览器”。



所有的非本地对象，所有的 BOM 和 DOM 对象都是宿主对象，如浏览器自带的document，window 等对象



