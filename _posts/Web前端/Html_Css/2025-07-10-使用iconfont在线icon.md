---
title: 使用iconfont在线icon
date: 2025-07-10
description: 使用iconfont在线icon
tags: [html-css]
categories: [Web前端, html-css]
---
# 使用iconfont在线icon

为了节省空间，也可以在线使用

在iconfont官网中**我的**页面创建一个自己的项目



![1667266642314-d0f37a57-1fc4-422c-abaf-668a09d6efdc.png](/assets/img/posts/Web前端/html-css/1667266642314-d0f37a57-1fc4-422c-abaf-668a09d6efdc-621712.png)



然后项目中引入 CSS

```css
@font-face {
  font-family: "iconfont"; /* Project id 2852827 */
  src: url('//at.alicdn.com/t/font_2852827_uy50eafxgg.woff2?t=1633673380229') format('woff2'),
    url('//at.alicdn.com/t/font_2852827_uy50eafxgg.woff?t=1633673380229') format('woff'),
    url('//at.alicdn.com/t/font_2852827_uy50eafxgg.ttf?t=1633673380229') format('truetype');
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-tupian:before {
  content: "\e64a";
}

.icon-xiaoshu:before {
  content: "\e7ef";
}

.icon-zhengshu:before {
  content: "\e7f2";
}

.icon-riqi:before {
  content: "\e65e";
}

.icon-biaoqian:before {
  content: "\e63d";
}

.icon-shijianriqi:before {
  content: "\e6e4";
}

.icon-xiala:before {
  content: "\e63f";
}

.icon-fujianwenjianjia:before {
  content: "\e61f";
}

.icon-jiliandongxuanzeqi:before {
  content: "\e616";
}

.icon-input:before {
  content: "\e604";
}

.icon-textarea:before {
  content: "\e60a";
}

.icon-time:before {
  content: "\e62b";
}

.icon-duoxuanzu:before {
  content: "\e64c";
}

```



然后通过普通标签显示

```html
<i class="iconfont icon-tupian"></i>
<i class="iconfont icon-jiliandongxuanzeqi"></i>
```



