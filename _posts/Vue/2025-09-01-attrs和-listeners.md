---
title: attrs和-listeners
date: 2025-09-01
description: $attrs 和 $listeners
tags: [Vue]
categories: [Vue]
---
# $attrs 和 $listeners

<code>**v-model**</code> 其实是个语法糖，它实际上是做了两步动作：

**1、绑定数据value**

**2、触发输入事件input**

***

vue中 自定义组件的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件



<code>**$attrs**</code>** **

包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (`class` 和 `style` 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (`class` 和 `style` 除外)，并且可以通过 `v-bind="$attrs"` 传入内部组件——在创建高级别的组件时非常有用。

> 所谓的$arrts 其实就是多级组件中的props，它就像一个中间件，用来传递爷组件给孙组件的数据，使用的时候只需给父组件中的孙组件配置v-bind="$attrs",然后再爷组件中传入孙组件所需要的数据，孙组件正常接收即可。

<code>**$listeners**</code>

包含了父作用域中的 (不含 `.native` 修饰器的) `v-on` 事件监听器。它可以通过 `v-on="$listeners"` 传入内部组件——在创建更高层次的组件时非常有用。

>  所谓的$listeners其实就相当于一个中间件，当出现多级组件嵌套时，孙组件想传递数据给爷组件，那么就需要在父组件中给孙组件设置 v-on="$listeners" ，然后爷组件通过@键 的方式监听孙组件传递过来的数据


