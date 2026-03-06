---
title: NPMinstall-save和-save-dev完美解释
date: 2025-04-25
description: NPM install -save 和 -save-dev 完美解释
tags: [开发整理, npm]
categories: [开发整理]
---
# NPM install -save 和 -save-dev 完美解释

最近在写Vue程序的时候，突然对 npm install 的-save和-save-dev 这两个参数的使用比较混乱。其实博主在这之前对这两个参数的理解也是模糊的，各种查资料和实践后对它们之间的异同点略有理解。遂写下这篇文章避免自己忘记，同时也给猿友一点指引。

我们在使用 **npm install** 安装模块的模块的时候 ，一般会使用下面这几种命令形式：



### 基本用法基本用法


```javascript
npm install moduleName 
# 安装模块到项目目录下

npm install -g moduleName 
# -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。

npm install -save moduleName 
# -save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。

npm install -save-dev moduleName 
# -save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
```



那么问题来了，在项目中我们应该使用四个命令中的哪个呢？这个就要视情况而定了。下面对这四个命令进行对比，看完后你就不再这么问了。



**npm install moduleName** 命令



1. 安装模块到项目node_modules目录下。
2. 不会将模块依赖写入devDependencies或dependencies 节点。
3. 运行 npm install 初始化项目时不会下载模块。



**npm install -g moduleName** 命令



1. 安装模块到全局，不会在项目node_modules目录中保存模块包。
2. 不会将模块依赖写入devDependencies或dependencies 节点。
3. 运行 npm install 初始化项目时不会下载模块。



**npm install -save moduleName** 命令



1. 安装模块到项目node_modules目录下。
2. 会将模块依赖写入dependencies 节点。
3. 运行 npm install 初始化项目时，会将模块下载到项目目录下。
4. 运行npm install --production或者注明NODE_ENV变量值为production时，会自动下载模块到node_modules目录中。



**npm install -save-dev moduleName** 命令



1. 安装模块到项目node_modules目录下。
2. 会将模块依赖写入devDependencies 节点。
3. 运行 npm install 初始化项目时，会将模块下载到项目目录下。
4. 运行npm install --production或者注明NODE_ENV变量值为production时，不会自动下载模块到node_modules目录中。



### 总结


**devDependencies** 节点下的模块是我们在开发时需要用的，比如项目中使用的 **webpack** ，压缩css、js的模块。这些模块在我们的项目部署后是不需要的，所以我们可以使用 **-save-dev** 的形式安装。像 **vuex** 这些模块是项目运行必备的，应该安装在 **dependencies** 节点下，所以我们应该使用 **-save** 的形式安装。



### 扩展


#### 命令缩写


在使用 npm install 命令时，有许多指定参数的命令是可以进行缩写的，本文就简单梳理一下。  
npm install本身有一个别名，即**npm i**，可以使用这种缩写方式来运行命令，打到简化的效果。  
以下为指定的一些命令行参数的缩写方式：



+ -g  
--global，缩写为-g，表示安装包时，视作全局的包。安装之后的包将位于系统预设的目录之下，一般来说
+ -S  
--save，缩写为-S，表示安装的包将写入package.json里面的dependencies。
+ -D  
--save-dev，缩写为-D，表示将安装的包将写入packege.json里面的devDependencies。
+ -O  
--save-optional缩写为-O，表示将安装的包将写入packege.json里面的optionalDependencies。
+ -E  
--save-exact缩写为-E，表示安装的包的版本是精确指定的。
+ -B  
--save-bundle缩写为-B，表示将安装的包将写入packege.json里面的bundleDependencies。



#### 淘宝镜像


```javascript
npm install -g cnpm --registry=https://registry.npm.taobao.org

//以后下载依赖直接用cnpm替代npm-享受飞一般的速度
```



#### 卸载模块


```javascript
npm uninstall express
```



#### 更新模块


```javascript
npm update express
```





