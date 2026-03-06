---
title: Android-IOS名称_图标
date: 2026-01-12
description: Android & IOS 名称/图标
tags: [Dart&Flutter, Flutter, android, ios]
categories: [Dart&Flutter, Flutter]
---
# Android & IOS 名称/图标

## 🤖Android

### 名称修改

<font style="color:rgb(51, 51, 51);">编辑</font><code><font style="color:rgb(51, 51, 51);background-color:rgb(237, 238, 240);">android/app/src/main/AndroidManifest.xml</font></code><font style="color:rgb(51, 51, 51);">文件，并在相应的</font><code><font style="color:rgb(51, 51, 51);background-color:rgb(237, 238, 240);"><application></font></code><font style="color:rgb(51, 51, 51);">标签中设置</font><code><font style="color:rgb(51, 51, 51);background-color:rgb(237, 238, 240);">android:label</font></code><font style="color:rgb(51, 51, 51);">属性：</font>

![1728702003737-a7824084-7822-40d2-9620-79aff0c959c0.png](/assets/img/posts/Dart&Flutter/1728702003737-a7824084-7822-40d2-9620-79aff0c959c0-864577.png)

### 图标替换

使用 [图标工厂](https://icon.wuruihong.com/) 自动生成各尺寸图标

![1728702151540-92fd2b59-3abd-4e80-a707-1ef79fe415af.png](/assets/img/posts/Dart&Flutter/1728702151540-92fd2b59-3abd-4e80-a707-1ef79fe415af-352538.png)

把生成好的图标替换掉：android/app/src/main/res/mipmap-hdpi/ic\_launcher.png

![1728721669495-0b49c62e-c1ac-4f4e-8540-355dcd59e323.png](/assets/img/posts/Dart&Flutter/1728721669495-0b49c62e-c1ac-4f4e-8540-355dcd59e323-896802.png)

## 🍎iOS

### 名称修改

请看下面链接介绍：

[Xcode14/15/16 设置Display Name不生效问题](https://www.yuque.com/hutaoao/blog/zies7duxzzxom6gu?singleDoc#)

### 图标替换

使用 [图标工厂](https://icon.wuruihong.com/) 自动生成各尺寸图标

![1728702151540-92fd2b59-3abd-4e80-a707-1ef79fe415af.png](/assets/img/posts/Dart&Flutter/1728702151540-92fd2b59-3abd-4e80-a707-1ef79fe415af-352538.png)

把生成好的图标替换掉 ios/Runner/Assets.xcassets/AppIcon.appiconset 即可：

![1728702291928-b092c2ab-b025-4ec4-ae5c-9c43d92ba40b.png](/assets/img/posts/Dart&Flutter/1728702291928-b092c2ab-b025-4ec4-ae5c-9c43d92ba40b-864833.png)


> 更新: 2024-10-12 16:28:41  
> 原文: <https://www.yuque.com/hutaoao/blog/mggpauzry256bq9y>