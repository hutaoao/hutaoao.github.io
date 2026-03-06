---
title: Homebrew安装nvm
date: 2025-06-11
description: Homebrew安装nvm
tags: [MacBookPro(M1)]
categories: [MacBookPro(M1)]
---
# Homebrew安装nvm

安装

```bash
brew install nvm
```



但是到这一步并没有安装好，这时直接使用nvm指令会得到：

```bash
nvm zsh: command not found: nvm
```

此时按照网上方法添加环境变量行不通，所以我们可以：

```bash
brew info nvm
```

如下图：按照提示添加环境变量即可

vi ~/.zshrc 进入编辑，最后 source ~/.zshrc 即可

![1625899942147-7122acd1-d8bd-4b85-a6b5-42f1366b006d.png](./img/4MjgF1WDAUKtky3_/1625899942147-7122acd1-d8bd-4b85-a6b5-42f1366b006d-232620.png)





node缓存目录 /Users/hutao/.nvm/.cache/



> 更新: 2024-06-24 10:41:24  
> 原文: <https://www.yuque.com/hutaoao/blog/zmvytu>