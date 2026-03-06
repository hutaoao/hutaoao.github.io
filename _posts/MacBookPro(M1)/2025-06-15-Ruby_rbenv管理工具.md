---
title: Ruby_rbenv管理工具
date: 2025-06-15
description: Ruby/rbenv管理工具
tags: [MacBookPro(M1)]
categories: [MacBookPro(M1)]
---
# Ruby/rbenv管理工具

### gem

#### 什么是gem

Gem是一个管理Ruby库和程序的标准包，它通过Ruby Gem（如 <https://rubygems.org/> ）源来查找、安装、升级和卸载软件包，非常的便捷。

> 类似于node的npm

#### 常用命令

* $ gem --version (查看gem版本)
* $ gem update --system(更新gem)
* $ gem sources(查看数据源)
* $ gem sources --remove [https://rubygems.org/(](https://rubygems.org/\(\)删除数据源)
* $ gem sources -a [https://ruby.taobao.org/(](https://ruby.taobao.org/\(\)添加数据源)
* $ gem search 软件包关键字(搜索软件包)
* $ gem install 软件包名称(安装软件包)
* $ gem install cocoapods --pre(安装上一个版本软件包)
* $ gem uninstall 软件包名称(卸载安装包)
* $ gem list 查看已安装的所有gem包

### rbenv

Ruby的版本管理工具：管理Ruby的版本，ReactNative所需ruby版本都不一样，所以需要管理一下ruby的版本。



#### 安装

<code>brew install rbenv</code>



#### 常用命令

* `rbenv install x.x.x`  	安装指定ruby版本
* `rbenv uninstall x.x.x` 	卸载指定ruby版本
* `rbenv install -l`       	列出最新的稳定版本
* `rbenv install -L`       	列出所有本地版本
* `rbenv global x.x.x`		设置本机的默认 Ruby 版本
* `rbenv local x.x.x`		设置该目录的 Ruby 版本
*
* `rbenv version`			显示当前活动的 Ruby 版本，以及有关其设置方式的信息
* `rbenv versions`			列出 rbenv 已知的所有 Ruby 版本，并在当前活动版本旁边显示一个星号


