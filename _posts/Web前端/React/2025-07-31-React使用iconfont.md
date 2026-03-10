---
title: React使用iconfont
date: 2025-07-31
description: React使用iconfont
tags: [React, react]
categories: [Web前端, React]
---
# React使用iconfont

单个图标用户可以自行选择下载不同的格式使用，包括 png、ai、svg。

点击下载按钮，可以选择下载图标。

![1747642775335-b985b07e-c18b-4b2f-894b-ae0bdfad8f02.png](/assets/img/posts/Web前端/React/1747642775335-b985b07e-c18b-4b2f-894b-ae0bdfad8f02-833882.png)

此种方式适合用在图标引用特别少，以后也不需要特别维护的场景。

* 比如设计师用来做demo原型。
* 前端临时做个活动页。
* 当然如果你只是为了下载图标做PPT,也是极好的。

不过如果是成体系的应用使用，建议用户把icon加入项目，然后使用下面三种推荐的方式。

## unicode 引用

***

unicode 是字体在网页端最原始的应用方式，特点是：

* 兼容性最好，支持 IE6+，及所有现代浏览器。
* 支持按字体的方式去动态调整图标大小，颜色等等。

*注意：新版iconfont支持彩色字体图标，兼容所有现代浏览器。详见：*[*https://www.iconfont.cn/help/article\_detail?article\_id=7*](https://www.iconfont.cn/help/article_detail?article_id=7)

unicode使用步骤如下：

### 第一步：拷贝项目下面生成的font-face

```css
@font-face {font-family: 'iconfont';
  src: url('iconfont.eot');
  src: url('iconfont.eot?#iefix') format('embedded-opentype'),
  url('iconfont.woff') format('woff'),
  url('iconfont.ttf') format('truetype'),
  url('iconfont.svg#iconfont') format('svg');
}
```

### 第二步：定义使用iconfont的样式

```css
.iconfont{
  font-family:"iconfont" !important;
  font-size:16px;font-style:normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;}
```

### 第三步：挑选相应图标并获取字体编码，应用于页面

```html
<i class="iconfont">&#x33;</i>
```

## font-class 引用

***

font-class是unicode使用方式的一种变种，主要是解决unicode书写不直观，语意不明确的问题。

与unicode使用方式相比，具有如下特点：

* 兼容性良好，支持ie8+，及所有现代浏览器。
* 相比于unicode语意明确，书写更直观。可以很容易分辨这个icon是什么。
* 因为使用class来定义图标，所以当要替换图标时，只需要修改class里面的unicode引用。
* 不过因为本质上还是使用的字体，所以多色图标还是不支持的。

使用步骤如下：

### 第一步：拷贝项目下面生成的fontclass代码：

```css
//at.alicdn.com/t/font_8d5l8fzk5b87iudi.css
```

### 第二步：挑选相应图标并获取类名，应用于页面：

```html
<i class="iconfont icon-xxx"></i>
```

## symbol 引用

***

这是一种全新的使用方式，应该说这才是未来的主流，也是平台目前推荐的用法。相关介绍可以参考这篇[文章](https://www.iconfont.cn/help/detail?spm=a313x.manage_type_myprojects.i1.d8cf4382a.2ab83a81839Ntf\&helptype=code) 这种用法其实是做了一个svg的集合，与上面两种相比具有如下特点：

* 支持多色图标了，不再受单色限制。
* 通过一些技巧，支持像字体那样，通过<code>font-size</code>,<code>color</code>来调整样式。
* 兼容性较差，支持 ie9+,及现代浏览器。
* 浏览器渲染svg的性能一般，还不如png。

使用步骤如下：

### 第一步：拷贝项目下面生成的symbol代码：

```css
//at.alicdn.com/t/font_8d5l8fzk5b87iudi.js
```

### 第二步：加入通用css代码（引入一次就行）：

```css
<style type="text/css">
.icon {
  width: 1em; height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>
```

### 第三步：挑选相应图标并获取类名，应用于页面：

```html
<svg class="icon" aria-hidden="true">
  <use xlink:href="#icon-xxx"></use>
</svg>
```

## React组件封装

将下载的文件放到 /assets/iconfont 目录下：

```jsx
import {type FC, memo} from 'react';
import '@/assets/iconfont/iconfont.js';
import '@/assets/iconfont/iconfont.css';

/***
 * name: 去图标库查看
 * iconfont 字体图标封装。图标库：xxxx
 */

// 使用方式
// <IconFont
//   name="icon-a-warn718"
{% raw %}
//   style={{
//     fontSize: '1rem',
//     color: '#9063d4',
//   }}
{% endraw %}
// />

const IconFont: FC<{ name: string; style?: React.CSSProperties }> = ({name, ...p}) => {
  return (
    <svg className="icon-font" {...p} style={p.style} aria-hidden="true">
      <use xlinkHref={'#' + name}/>
    </svg>
  );
};

export default memo(IconFont);
```

全局CSS文件中加入：

```css
.icon-font {
  width: 1em;
  height: 1em;
  fill: currentColor;
  overflow: hidden;
}
```


