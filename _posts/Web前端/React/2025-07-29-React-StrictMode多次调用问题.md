---
title: React-StrictMode多次调用问题
date: 2025-07-29
description: React.StrictMode 多次调用问题
tags: [React, react]
categories: [Web前端, React]
---
# React.StrictMode 多次调用问题

百度搜“StrictMode多次渲染”，很快发现大家早就知道了这个问题，浏览了几篇，大概是说在 StrictMode 下，组件的一些方法会被调用多次，了解原因后，于是去翻 官方文档 加以确认：



> 严格模式不能自动检测到你的副作用，但它可以帮助你发现它们，使它们更具确定性。通过故意重复调用以下函数来实现的该操作：
> 
> class 组件的 constructor，render 以及 shouldComponentUpdate 方法  
class 组件的生命周期方法 getDerivedStateFromProps  
函数组件体  
状态更新函数 (即 setState 的第一个参数）  
函数组件通过使用 useState，useMemo 或者 useReducer

  
确认了 函数组件在 StrictMode 下会被重复调用，至此还剩下一个小问题：既然组件函数被多调用了一次，那么 console.log 应该成对出现，为啥同步的 console.log 会少一个？继续看文档：



> 注意：  
从 React 17 开始，React 会自动修改 console 的方法，例如 console.log()，以在对生命周期函数的第二次调用中静默日志。然而，在某些可以使用替代解决方案的情况下，这可能会导致一些不期望的行为的发生。



原来 console 被 React 修改了，好吧。



**总结**  
在 React.StrictMode 下，React 通过重复调用组件的一些钩子，从而使副作用更容易暴露出来；同时，它仅在开发环境中运行，不会影响生产构建。



