---
title: vue-webpakeproxyTable跨域问题完美解决
date: 2025-09-12
description: vue+webpake proxyTable跨域问题完美解决
tags: [vue, web]
categories: [Web前端, Vue]
---
# vue+webpake proxyTable跨域问题完美解决





如果是前后端分离的项目，本地开发环境需要访问接口，或者在调试时直接访问线上的接口，这时候就会有接口跨域的问题（生产环境不存在跨域问题）。以vue-cli生成的项目为例，需要配置 config/index.js 中的 proxyTable 属性。

### 方法一


```javascript
/* 亲测只适合dev环境，且每个接口前要加上‘/api’ */

proxyTable: {
    '/api': {
        target: 'http://example.com', // 接口的域名
        // secure: false,  // 如果是https接口，需要配置这个参数
        changeOrigin: true, // 如果接口跨域，需要进行这个参数配置
        pathRewrite: {
            '^/api': ''
        }
    }
}
```



### 方法二


```javascript
/* 亲测完美解决跨域方案 */

proxyTable:[{
    path: '/',
    target: 'http://example.com', // 接口的域名
}],
```



![1574644847812-59c46b2e-447e-47fc-8e11-61659d58c204.png](/assets/img/posts/Web前端/Vue/1574644847812-59c46b2e-447e-47fc-8e11-61659d58c204-694887.png)



### 最新方法
vue-cli3 创建的项目

在vue.config.js里面配置如下图即可：

![1615429335582-6ff5f477-b6d4-4e02-b93e-20d4dcb8b8a9.png](/assets/img/posts/Web前端/Vue/1615429335582-6ff5f477-b6d4-4e02-b93e-20d4dcb8b8a9-658042.png)



