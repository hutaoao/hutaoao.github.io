---
title: rbenv-ruby版本管理
date: 2025-06-17
description: rbenv - ruby版本管理
tags: [MacBookPro(M1)]
categories: [MacBookPro(M1)]
---
# rbenv - ruby版本管理

### 安装

1. `brew install rbenv`
2. `rbenv init`

### 使用

* rbenv install--list	# 列出所有 ruby 版本
* rbenv install 3.0.4 	# 安装 3.0.4
* rbenv versions 		# 列出安装的版本
* rbenv version 		# 列出正在使用的版本
* **<font style="color:#DF2A3F;">rbenv global</font>** 3.0.4 	# 默认使用 3.0.4
* rbenv shell 3.0.4 	# 当前的 shell 使用 3.0.4, 会设置一个 `RBENV_VERSION` 环境变量
* rbenv local 3.0.4 	# 当前目录使用 3.0.4, 会生成一个 `.rbenv-version` 文件

### 环境变量

> export PATH="/Users/hutao/.rbenv/shims:$PATH"
> export PATH="/Users/hutao/.rbenv/bin:$PATH"

source .zshrc


