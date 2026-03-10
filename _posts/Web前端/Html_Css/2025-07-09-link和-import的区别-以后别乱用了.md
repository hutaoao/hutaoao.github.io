---
title: link和-import的区别-以后别乱用了
date: 2025-07-09
description: link和@import的区别，以后别乱用了！
tags: [Html、Css]
categories: [Web前端, Html_Css]
---
# link和@import的区别，以后别乱用了！

我们都知道，外部引入 CSS 有2种方式，**link**标签和**@import  **。



### 1.属性不同


@import是 CSS 提供的语法规则，只有导入样式表的作用；link是HTML提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性等。



### 2.加载顺序不同


加载页面时，link标签引入的 CSS 被同时加载；@import引入的 CSS 将在页面加载完毕后被加载。



### 3.兼容性不同


@import是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；link标签作为 HTML 元素，不存在兼容性问题。



### 4.DOM可控性区别


可以通过 JS 操作 DOM ，插入link标签来改变样式；由于 DOM 方法是基于文档的，无法使用@import的方式插入样式。



### 5.权重区别(该项有争议)


link引入的样式权重大于@import引入的样式。



**结论：**



就结论而言，强烈建议使用link标签，慎用@import方式。  
这样可以避免考虑@import的语法规则和注意事项，避免产生资源文件下载顺序混乱和http请求过多的烦恼



