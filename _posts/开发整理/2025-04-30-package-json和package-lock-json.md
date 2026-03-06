---
title: package-json和package-lock-json
date: 2025-04-30
description: package.json和package-lock.json
tags: [开发整理]
categories: [开发整理]
---
# package.json和package-lock.json

### 引言
我们在搭建项目的时候，通过 npm 安装的依赖模块时，package.json文件中依赖的版本号前面会带符号 ^，有时候我们看别人的项目时也可能会看版本前带符号 ~ ，或者什么也不带，其中会有什么区别呢？而且当你的 npm 版本升级到 5.X.X 版本以上的时候，对应目录下还是自动生成一个 package-lock.json 文件（cnpm不支持package-lock.json），这个文件的作用又是什么呢。博主根据网上资料简单说明一下。



### package.json


```json
"dependencies": {
  "react": "^16.8.0"
  "react": "~16.8.0",
  "react": "16.8.0",
},
```

如上，三种方式的区分在于，项目通过 npm install 重新下载依赖包时，对于所下载的版本号的区别：



+ ‘^16.8.0’ 表示安装16.x.x的最新版本，安装时不改变大版本号。
+ ‘~16.8.0’ 表示安装16.8.x的最新版本，安装时不改变大版本号和次要版本号。
+ ‘16.8.0’ 表示安装指定的版本号，也就是安装16.8.0版本。

我们可以通过



```bash
npm view react versions
```



来查看到目前我们所能获取到的所有react的版本,这里从16.8.0开始截止，目前最新的是16.12.0

```bash
'16.8.0',
'16.8.1',
'16.8.2',
'16.8.3',
'16.8.4',
'16.8.5',
'16.8.6',
'16.9.0-alpha.0',
'16.9.0-rc.0',
'16.9.0',
'16.10.0',
'16.10.1',
'16.10.2',
'16.11.0',
'16.12.0'
```



以这个为例，上述的三种写法，最终install的版本号分别为：

+ ‘^16.8.0’ 安装16.12.0
+ ‘~16.8.0’ 安装16.8.6
+ ‘16.8.0’ 安装16.8.0



所以，上面三种写法除了 16.8.0 这种，其他两种都有可能实际安装的版本号与 package.json 文件中的版本号不一致。我们可以通过：

```bash
npm view react version
```



来查看目前安装的react的具体版本号。正常情况下，依赖包的版本都是向下兼容的，所以通过^16.8.0 和~16.8.0这两种方式也很少报错。npm 默认的带^ ，你可以通过 npm config set save-prefix='~' 将 ~ 设置为默认符号。



### package-lock.json
package-lock.json 的作用就是为了避免上述的 package.json 中的版本号与实际安装的不一致的问题。package-lock.json 在 npm 版本为 5 以上时，会自动生成。



官方的解释是：

> package-lock.json is automatically generated for any operations where npm modifies either the node_modules tree, or package.json. It describes the exact tree that was generated, such that subsequent installs are able to generate identical trees, regardless of intermediate dependency updates.



通俗的说就是：package-lock.json里会维护一个依赖管理树，里面记录着每个依赖的确定版本, 获取地址和哈希值等信息，这样就保证了每次安装下载的依赖版本都是一样的。



如下是 package-lock.json 中的 react 的相关信息：

```json
"react": {
    "version": "16.12.0",
    "resolved": "https://registry.npmjs.org/react/-/react-16.12.0.tgz",
    "integrity": "sha512-fglqy3k5E+81pA8s+7K0/T3DBCF0ZDOher1elBFzF7O6arXJgzyu/FW+COxFvAWXJoJN9KIZbT2LXlukwphYTA==",
    "requires": {
    "loose-envify": "^1.1.0",
    "object-assign": "^4.1.1",
    "prop-types": "^15.6.2"
    }
},
```



如果想要更新依赖的版本号，需要使用 npm install xxx@1.0.0 --save 这种方式来进行版本更新，这样 package-lock.json 文件才可以中也会对应做更新。





