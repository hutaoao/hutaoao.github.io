---
title: git生成并添加SSHkey
date: 2025-07-17
description: git生成并添加SSH key
tags: [Git, git]
categories: [Git]
---
# git生成并添加SSH key

**1、执行以下命令：**



①   cd ~/.ssh/    【如果没有对应的文件夹，则执行  mkdir  ./.ssh】



②  git config --global user.name "xb12369"



③  git config --global user.email "1234@qq.com"



④  ssh-keygen -t rsa -C "1234@qq.com"

![FE6569B73A54400594E2422057FD1072](http://note.youdao.com/yws/res/895/FE6569B73A54400594E2422057FD1072?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_aHV0YW9hbw%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



**2、找到C:\Users\xb12369\.ssh 目录，里面有两个文件：id_rsa和id_rsa.pub**



**3、配置ssh【这里是id_rsa.pub里面的内容啊】**







> 更新: 2020-03-16 14:08:26  
> 原文: <https://www.yuque.com/hutaoao/blog/szks5u>