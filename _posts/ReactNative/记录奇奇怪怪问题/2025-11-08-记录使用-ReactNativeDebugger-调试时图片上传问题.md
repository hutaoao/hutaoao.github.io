---
title: 记录使用-ReactNativeDebugger-调试时图片上传问题
date: 2025-11-08
description: 记录使用“React Native Debugger”调试时图片上传问题
tags: [ReactNative, 记录奇奇怪怪问题, reactnative, react, native]
categories: [ReactNative, 记录奇奇怪怪问题]
---
# 记录使用“React Native Debugger”调试时图片上传问题

在使用“React Native Debugger”调试时图片上传时：

从network中发现`formdata`格式的file变为了` [object object]`

![1723599795375-6256b61d-621c-4444-aef2-79e341227e8c.png](/assets/img/posts/ReactNative/1723599795375-6256b61d-621c-4444-aef2-79e341227e8c-140024.png)

就导致上传失败 接口500，服务端没有接受到文件。

中间总以为自己的程序哪里出了问题（之前都好好的，突然不行了）

后来发现就是因为使用了“React Native Debugger”调试导致的

使用浏览器的`http://localhost:8081/debugger-ui/`调试没有问题的

真实打包到生产环境也没有问题的。

虚惊一场！！！


