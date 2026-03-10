---
title: sessionStorage误区解释
date: 2025-10-19
description: sessionStorage误区解释
tags: [javascript]
categories: [Web前端, JavaScript]
---
# sessionStorage误区解释

# 1、前言

所有人都知道，`localStorage`和`sessionStorage`的最大区别是生命周期，一个永久，一个仅针对一个会话期间有效。那么，到底什么是一个会话？多个标签页之间的数据是否会共享呢？

# 2、后台的session

我们对会话`session`的认识一般都是从后台的session开始的，比如Java的session，它是基于往cookie写入一个`JSESSIONID`来实现的，所以，只要你不是打开一个隐身窗口，无论你开多少个标签页，不同标签页之间都会被认为是一个session，你在这个标签页登录了，新开一个标签输入地址，仍然是登录状态。

# 3、sessionStorage的session

但是直到今天才发现，HTML5中的这个`sessionStorage`和传统后台的`session`并不完全是同一个东西，主要是在多个标签页数据是否会共享的问题上的不同。

误区：之前一直以为，同一个窗口，只要会话还没有过期，不同标签页之间，相同域名下的`sessionStorage`是一样的。

正确答案：刷新当前页面，或者通过`location.href`、`window.open`、或者通过带`target="_blank"`的`a`标签打开新标签，之前的`sessionStorage`还在（继承过去了），但是如果你是主动打开一个新窗口或者新标签，对不起，打开F12你会发现，`sessionStorage`空空如也。

也就是说，`sessionStorage`的`session`仅限当前标签页或者当前标签页打开的新标签页，通过其它方式新开的窗口或标签不认为是同一个`session`。

大家可以亲自测试一下，手动打开的新标签和点A标签打开的新标签效果完全不一样。

![1584608752458-e6e9f43b-ac2c-44a2-86df-363c6a3bfaaf.gif](/assets/img/posts/Web前端/JavaScript/1584608752458-e6e9f43b-ac2c-44a2-86df-363c6a3bfaaf-048708.gif)

# 20180522更新

今天又碰到有关sessionStorage的一个问题，发现之前理解的还是错误的，比如当我通过A标签打开新的窗口时，在新窗口删除同样的数据，旧窗口的却还在。

经过测试发现：

> 通过带`target="_blank"`的A标签、window.open等方式打开新窗口时，会把旧窗口（或标签）的sessionStorage数据带过去，但从此之后，新窗口（或标签）的sessionStorage的增删改和旧窗口已经没有关系了，如果只是在当前标签内跳转新页面（或者刷新），数据还会保留（前提当然是同域）。

总之，在处理sessionStorage时，只要打开新窗口就要特别注意了，新旧窗口数据不会互相同步。


