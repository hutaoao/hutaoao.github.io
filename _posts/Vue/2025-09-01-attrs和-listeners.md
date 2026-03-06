---
title: attrs和-listeners
date: 2025-09-01
description: $attrs 和 $listeners
tags: [Vue]
categories: [Vue]
---
# $attrs 和 $listeners

<code>**<font style="color:#F5222D;">v-model</font>**</code> 其实是个语法糖，它实际上是做了两步动作：

**1、绑定数据value**

**2、触发输入事件input**

***

<font style="color:#304455;">vue中 </font>自定义组件<font style="color:#304455;">的 </font>`v-model`<font style="color:#304455;"> 默认会利用名为 </font>`value`<font style="color:#304455;"> 的 prop 和名为 </font>`input`<font style="color:#304455;"> 的事件</font>

<font style="color:#304455;"></font>

<code>**<font style="color:#F5222D;">$attrs</font>**</code>**<font style="color:#F5222D;"> </font>**

<font style="color:#304455;">包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (</font>`class`<font style="color:#304455;"> 和 </font>`style`<font style="color:#304455;"> 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (</font>`class`<font style="color:#304455;"> 和 </font>`style`<font style="color:#304455;"> 除外)，并且可以通过 </font>`v-bind="$attrs"`<font style="color:#304455;"> 传入内部组件——在创建高级别的组件时非常有用。</font>

> 所谓的$arrts 其实就是多级组件中的props，它就像一个中间件，用来传递爷组件给孙组件的数据，使用的时候只需给父组件中的孙组件配置v-bind="$attrs",然后再爷组件中传入孙组件所需要的数据，孙组件正常接收即可。

<code>**<font style="color:#F5222D;">$listeners</font>**</code>

<font style="color:#304455;">包含了父作用域中的 (不含 </font>`.native`<font style="color:#304455;"> 修饰器的) </font>`v-on`<font style="color:#304455;"> 事件监听器。它可以通过 </font>`v-on="$listeners"`<font style="color:#304455;"> 传入内部组件——在创建更高层次的组件时非常有用。</font>

>  所谓的$listeners其实就相当于一个中间件，当出现多级组件嵌套时，孙组件想传递数据给爷组件，那么就需要在父组件中给孙组件设置 v-on="$listeners" ，然后爷组件通过@键 的方式监听孙组件传递过来的数据


> 更新: 2022-11-16 16:03:34  
> 原文: <https://www.yuque.com/hutaoao/blog/ofux7fdzir3gzc37>