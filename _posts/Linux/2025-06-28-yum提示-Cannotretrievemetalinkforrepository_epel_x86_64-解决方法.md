---
title: yum提示-Cannotretrievemetalinkforrepository_epel_x86_64-解决方法
date: 2025-06-28
description: "yum提示\"Cannot retrieve metalink for repository: epel/x86_64\" 解决方法"
tags: [Linux]
categories: [Linux]
---
# yum提示"Cannot retrieve metalink for repository: epel/x86_64" 解决方法

<font style="color:rgb(68, 68, 68);">今天在centos7服务器上用yum的时候发现，yum命令不能用了，不管用yum什么命令都会出现如下提示：</font>



![1646898704438-b862ce6d-6459-4f75-936f-08c2402966b4.png](/assets/img/posts/Linux/1646898704438-b862ce6d-6459-4f75-936f-08c2402966b4-559516.png)



<font style="color:rgb(68, 68, 68);">按照网上很多资料都没有解决我的问题，因为：</font>

<font style="color:rgb(68, 68, 68);">1、我的服务器 ping 正常的，无论是ping镜像还是ping其它网站都可以ping通。</font>

<font style="color:rgb(68, 68, 68);">2、/etc/resolv.conf 里面的 nameserver 114.114.114.114 也是有的，dns也没问题。</font>

<font style="color:rgb(68, 68, 68);"></font>

## <font style="color:rgb(68, 68, 68);">解决方法</font>
<font style="color:rgb(68, 68, 68);">出现此问题，最根本的原因是"/etc/yum.repos.d/epel.repo"里面的镜像源路径不对。</font>

<font style="color:rgb(68, 68, 68);">既然找到了原因，解决起来就容易多了，具体步骤如下：</font>

<font style="color:rgb(68, 68, 68);"></font>

<font style="color:rgb(68, 68, 68);">1、打开 /etc/yum.repos.d/epel.repo；</font>

```bash
vi /etc/yum.repos.d/epel.repo
```

<font style="color:rgb(68, 68, 68);"></font>

<font style="color:rgb(68, 68, 68);">2、注释掉mirrorlist，取消注释baseurl；</font>

<font style="color:rgb(68, 68, 68);"></font>

<font style="color:rgb(68, 68, 68);">将</font>

<font style="color:rgb(102, 102, 102);">[epel]</font><font style="color:rgb(68, 68, 68);"> </font>

<font style="color:rgb(68, 68, 68);">name=Extra Packages for Enterprise Linux </font><font style="color:rgb(102, 102, 102);">7</font><font style="color:rgb(68, 68, 68);"> - </font><font style="color:rgb(102, 102, 102);">$basearch</font><font style="color:rgb(68, 68, 68);"> </font><font style="color:rgb(102, 102, 102);">#baseurl=https://download.fedoraproject.org/pub/epel/7/$basearch</font><font style="color:rgb(68, 68, 68);"> </font>metalink<font style="color:rgb(68, 68, 68);">=https://mirrors.fedoraproject.org/metalink?repo=epel-</font><font style="color:rgb(102, 102, 102);">7</font><font style="color:rgb(68, 68, 68);">&arch=</font><font style="color:rgb(102, 102, 102);">$basearch</font>

<font style="color:rgb(68, 68, 68);">修改为</font>

<font style="color:rgb(102, 102, 102);">[epel]</font>

<font style="color:rgb(68, 68, 68);">name=Extra Packages for Enterprise Linux </font><font style="color:rgb(102, 102, 102);">7</font><font style="color:rgb(68, 68, 68);"> - </font><font style="color:rgb(102, 102, 102);">$basearch</font><font style="color:rgb(68, 68, 68);"> baseurl=https://download.fedoraproject.org/pub/epel/</font><font style="color:rgb(102, 102, 102);">7</font><font style="color:rgb(68, 68, 68);">/</font><font style="color:rgb(102, 102, 102);">$basearch</font><font style="color:rgb(68, 68, 68);"> </font><font style="color:rgb(102, 102, 102);">#</font>metalink<font style="color:rgb(102, 102, 102);">=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch</font><font style="color:rgb(68, 68, 68);"> </font>

<font style="color:rgb(68, 68, 68);"></font>

![1646899313434-6d8426b9-1afc-4c1b-9fe1-6a9aa091f302.png](/assets/img/posts/Linux/1646899313434-6d8426b9-1afc-4c1b-9fe1-6a9aa091f302-894211.png)

<font style="color:rgb(68, 68, 68);">3、再次使用yum命令即正常！</font>



**推荐切换国内源**  




> 更新: 2026-03-06 11:32:19  
> 原文: <https://www.yuque.com/hutaoao/blog/ih4k4w>