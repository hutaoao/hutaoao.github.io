---
title: axios-creat创建示例
date: 2025-08-04
description: axios-creat 创建示例
tags: [react, ios]
categories: [Web前端, React]
---
# axios-creat 创建示例

当项目中请求多个 `apis` 时，且有可能每个 `apis` 里面配置都不同（含拦截器内容不同），此时会产生叠加效果，此时需要使用 *axios.creat(config)* 来创建示例分别配置。

### 场景

如下图：该项目中请求了 Java、Php 等不同的接口，每个服务端接口配置可能不同

![1605766549400-632e5f63-5b3b-458f-98de-bc038dca126c.jpeg](/assets/img/posts/Web前端/React/1605766549400-632e5f63-5b3b-458f-98de-bc038dca126c-110916.jpeg)

### 使用

* 创建实例： axios.create()

```javascript
axios.create({
    baseURL: baseUrl,//请求基地址
    timeout: 3000,//请求超时时长
    url: '/url',请求路径
    method: 'get,post,put,patch,delete',//请求方法
    headers: {
        token: ''
    },//请求头
    params: {},//请求参数拼接在url上面
    data: {},//请求参数放请求体里
})
```

* 参数配置位置

1. 全局配置（**优先级最低**）

```javascript
axios.default.timeout = 3000
axios.default.baseURL = 3000
```

2. 实例配置

```javascript
let instance = axios.create()
instance.default.timeout = 1000
```

3. axios请求时配置（**优先级最高**）

```javascript
instance.get('/url', {
    timeout: 5000
})
```

### 实际开发

* 多个apis

```javascript
import http from 'axios';
const $http = http.create();

let url = '';
if (process.env.REACT_APP_SECRET_CODE === 'dev') {//测试环境
    url = 'http://test.cn';
} else if (process.env.REACT_APP_SECRET_CODE === 'pre') {//预发环境
    url = 'http://pre.cn';
} else if (process.env.REACT_APP_SECRET_CODE === 'prd') {//线上环境
    url = 'http://prd.cn';
}
> 这里我项目中使用到了环境变量 区分不同环境

let URL = process.env.NODE_ENV === 'development' ? '/phpApi' : url;

// 请求拦截器
$http.interceptors.request.use(config => {
    // 每次发送请求之前判断是否存在token，如果存在，则统一在http请求的header都加上token，不用每次请求都手动添加了
    // 即使本地存在token，也有可能token是过期的，所以在响应拦截器中要对返回状态进行判断
    const token = sessionStorage.getItem('jwtToken');
    token && (config.headers.jwtToken = token);
    return config;
}, error => {
    return Promise.error(error);
});

//登录
export const unifiedLogin = params => {
    return $http.post(`${URL}/login`, params)
};
```

* 其它

```javascript
let instance = axios.create({
    baseURL: 'http://192.168.X.X:8080'
})

let instance2 = axios.create({
    baseURL: 'http://192.168.X.X:8081'
})

instance.get('/url',{
    timeout: 2000
}).then()

instance2.get('/url',{
    timeout: 3000
}).then()
```


