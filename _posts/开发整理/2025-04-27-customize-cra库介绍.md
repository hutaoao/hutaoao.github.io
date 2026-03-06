---
title: customize-cra库介绍
date: 2025-04-27
description: customize-cra库介绍
tags: [开发整理]
categories: [开发整理]
---
# customize-cra库介绍

<code>customize-cra</code> 是依赖于 <code>react-app-rewired</code> 的库，通过 <code>config-overrides.js</code> 来修改底层的 webpack，babel配置。



<code>config-overrides.js</code> 创建在项目根目录下，是 <code>react-app-rewird</code> 包所利用的文件，结合<code>customize-cra</code> 可以轻松修改底层配置，而不用运行 npm run eject 来暴露 webpack.config.js 来修改配置。

示例如下：

```javascript
const { override } = require('customize-cra');
module.exports = override({
    // 在这里使用 customize-cra 里的一些函数来修改配置
});
```

### addWebpackAlias

添加别名，设置相对路径。\ 创建 import 或 require 的别名，来确保模块引入变得更简单。例如，一些位于 src/ 文件夹下的常用模块。使用 customize-cra 中的 <code>addWebpackAlias</code> 模块来实现该功能。

```javascript
const { override, addWebpackAlias} = require('customize-cra');
const path = require('path');
module.exports = override(    
    addWebpackAlias({      
    		"@": path.resolve(__dirname, 'src')
    })
)
```

引入时：

```javascript
import containers from '@/containers';
//等同于：
import containers from 'src/containers';
import containers from '../src/containers';
import containers from '../../src/containers';
```

### addLessLoader

```javascript
const { override, addLessLoader } = require('customize-cra');
module.exports = override({
    // 调用这个函数相当于在 webpack.config.js 里面配置 less-loader  前提是要下载 less 和 less-loader,
    // 下面的配置不光配置了less-loader，还设置了less模块化语法，但是只能是以 .module.less 的文件才能模块化
    addLessLoader({
        lessOptions: {
           javascriptEnabled: true,
           localIdentName: '[local]--[hash:base64:5]'
        }
    }),
});
```

### fixBabelImports

```javascript
const { override, fixBabelImports } = require('customize-cra');
module.exports = override({
    // 这里以antd-mobile 按需下载为例， 使用这个方法前提是 安装了 babel-plugin-import 插件
    fixBabelImports('import', {
        libraryName: 'antd-mobile',
        style: 'css' // 加载组件时连样式一并加载
    }),
});
```

### addBabelPlugin

```javascript
const { override, fixBabelImports } = require('customize-cra');
module.exports = override({
    // addBabelPlugin 用来配置添加babel插件的 
    // 这里以 @babel/plugin-proposal-decorators 插件为例， 这个插件是用来支持 es7 装饰器语法的
    addBabelPlugin(
        ["@babel/plugin-proposal-decorators", { "legacy": true }]
    )
});
```


