---
title: ios更换应用图标testflight更新后返回桌面会闪一下旧的图标
date: 2025-11-05
description: ios更换应用图标testflight更新后返回桌面会闪一下旧的图标
tags: [ReactNative, 记录奇奇怪怪问题, ios]
categories: [ReactNative, 记录奇奇怪怪问题]
---
# ios更换应用图标testflight更新后返回桌面会闪一下旧的图标

### icon
现象（注意观察最后两到三秒）：

[https://qiniu.public.hutaoao.cn/RPReplay_Final1732007514.MP4](https://qiniu.public.hutaoao.cn/RPReplay_Final1732007514.MP4)



考虑应该是iOS系统升级或者testflight升级的某种缓存机制



**<font style="color:#DF2A3F;">重启设备</font>**<font style="color:#DF2A3F;">或</font>**<font style="color:#DF2A3F;">桌面切换</font>**下就能显示正常了 😓



### 启动页
启动页要是有缓存的话

上传的启动页图片换个命名

![1732009698133-2b522c75-6a2f-405c-9504-199622b72ed1.png](/assets/img/posts/ReactNative/1732009698133-2b522c75-6a2f-405c-9504-199622b72ed1-685356.png)



