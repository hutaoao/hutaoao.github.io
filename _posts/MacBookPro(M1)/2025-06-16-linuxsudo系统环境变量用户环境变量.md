---
title: linuxsudo系统环境变量用户环境变量
date: 2025-06-16
description: linux sudo 系统环境变量 用户环境变量
tags: [MacBookPro(M1), linux]
categories: [MacBookPro(M1)]
---
# linux sudo 系统环境变量 用户环境变量

1. sudo 就是普通用户临时拥有root的权限。好处在于，大多数时候使用用户自定义的配置，少数情况可以通过sudo实现root权限做事。

故而，需要注意的一点是，在你使用了sudo后，你临时不再是原先用户，不能使用属于自己的命令。举个例		  子：sudo source ... 该命令会执行失败，提示没有source命令。但你去掉sudo，又可以执行了。（从侧面可以 反映sudo不等于获得root所有权限。）莫要滥用sudo。



2.  系统环境变量，对应/etc/profile文件，对所有用户有效。而用户环境变量，对应~/.bashrc文件，仅仅对自己的用户有效。  
所以，在非root情况下，多数环境变量建议在~/.bashrc上操作。 



> M1 终端用的 zsh ,所以用户环境变量文件为 ~/.zshrc



