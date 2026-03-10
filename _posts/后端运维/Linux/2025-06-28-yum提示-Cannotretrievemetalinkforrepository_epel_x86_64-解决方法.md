---
title: yum提示-Cannotretrievemetalinkforrepository_epel_x86_64-解决方法
date: 2025-06-28
description: "yum提示\"Cannot retrieve metalink for repository: epel/x86_64\" 解决方法"
tags: [Linux]
categories: [后端运维, Linux]
---
# yum提示"Cannot retrieve metalink for repository: epel/x86_64" 解决方法

今天在centos7服务器上用yum的时候发现，yum命令不能用了，不管用yum什么命令都会出现如下提示：



![1646898704438-b862ce6d-6459-4f75-936f-08c2402966b4.png](/assets/img/posts/后端运维/Linux/1646898704438-b862ce6d-6459-4f75-936f-08c2402966b4-559516.png)



按照网上很多资料都没有解决我的问题，因为：

1、我的服务器 ping 正常的，无论是ping镜像还是ping其它网站都可以ping通。

2、/etc/resolv.conf 里面的 nameserver 114.114.114.114 也是有的，dns也没问题。



## 解决方法
出现此问题，最根本的原因是"/etc/yum.repos.d/epel.repo"里面的镜像源路径不对。

既然找到了原因，解决起来就容易多了，具体步骤如下：



1、打开 /etc/yum.repos.d/epel.repo；

```bash
vi /etc/yum.repos.d/epel.repo
```



2、注释掉mirrorlist，取消注释baseurl；



将

[epel] 

name=Extra Packages for Enterprise Linux 7 - $basearch #baseurl=https://download.fedoraproject.org/pub/epel/7/$basearch metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch

修改为

[epel]

name=Extra Packages for Enterprise Linux 7 - $basearch baseurl=https://download.fedoraproject.org/pub/epel/7/$basearch #metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch 



![1646899313434-6d8426b9-1afc-4c1b-9fe1-6a9aa091f302.png](/assets/img/posts/后端运维/Linux/1646899313434-6d8426b9-1afc-4c1b-9fe1-6a9aa091f302-894211.png)

3、再次使用yum命令即正常！



**推荐切换国内源**  




