---
title: Android-IOS启动页
date: 2026-01-13
description: Android & IOS 启动页
tags: [移动开发, flutter, android, ios]
categories: [移动开发, Flutter]
---
# Android & IOS 启动页

### [链接](about:blank)Ios

替换以下文件

![1712472617100-1ecb18d2-021e-4652-b292-49dd306a543f.png](/assets/img/posts/移动开发/Flutter/1712472617100-1ecb18d2-021e-4652-b292-49dd306a543f-115325.png)

或者直接修改：

![1713854099811-77e5d3dd-fbad-474f-bfe1-a5fae6ad2a94.png](/assets/img/posts/移动开发/Flutter/1713854099811-77e5d3dd-fbad-474f-bfe1-a5fae6ad2a94-733417.png)

然后打开Xcode：

点击屏幕右下角的约束编辑器：

将上面填空处都填 0，然后点击 Add 4 Constraints。

![1712472811748-07c857b1-3820-4943-83eb-d7a41ae1e6f6.png](/assets/img/posts/移动开发/Flutter/1712472811748-07c857b1-3820-4943-83eb-d7a41ae1e6f6-824699.png)

打开LaunchScreen，将 Content Mode 修改为 Scale To Fill：

![1712472713865-08f41054-0d06-4a72-bd1b-0d8d0a528a0e.png](/assets/img/posts/移动开发/Flutter/1712472713865-08f41054-0d06-4a72-bd1b-0d8d0a528a0e-587679.png)

保证这里都是`Superview`

![1713853996701-7f2f4db7-7030-4d34-bad7-bfe695755baa.png](/assets/img/posts/移动开发/Flutter/1713853996701-7f2f4db7-7030-4d34-bad7-bfe695755baa-508123.png)

重新运行就可以了。

### Android

如下图：

注释掉蓝色部分，放开红色部分；然后每个dpi文件夹下放入对应分辨率的启动页图片就可以了。

> android:gravity="**fill**" 这个是全屏覆盖的关键（图片可能会被拉伸）

![1712719552587-adc994f6-11f7-47df-bd4e-e14537dc7c3d.png](/assets/img/posts/移动开发/Flutter/1712719552587-adc994f6-11f7-47df-bd4e-e14537dc7c3d-953967.png)

#### 扩展：res目录下的 drawable/drawable-v21

:::warning 💁💁💁

****

安卓项目的res目录中有多个drawable文件夹\ 在Android项目的res目录中，可以有多个drawable文件夹，每个文件夹用于存放不同类型的可绘制资源。下面是常见的几个drawable文件夹及其区别：

**drawable**: 这是默认的drawable文件夹，用于存放通用的可绘制资源，例如图片、形状定义等。这些资源在所有屏幕密度下都会被使用。

drawable-hdpi, drawable-mdpi, drawable-xhdpi, drawable-xxhdpi, drawable-xxxhdpi: 这些文件夹分别用于存放不同屏幕密度下的可绘制资源。Android设备根据屏幕密度的不同选择合适的文件夹来加载资源，以确保在不同设备上显示效果一致。

**drawable-v21**: 这个文件夹用于存放针对Android 5.0（API级别21）及更高版本的特定版本的可绘制资源。当应用运行在 Android 5.0 及更高版本时，系统会优先加载这个文件夹下的资源。

drawable-land和drawable-port: 这两个文件夹用于存放横屏和竖屏模式下的可绘制资源。你可以将相应方向下的资源放置在这些文件夹中，系统会根据屏幕方向自动加载合适的资源。

除了上述常见的drawable文件夹外，你也可以根据需要创建其他命名格式的文件夹，例如drawable-en, drawable-fr等，用于存放不同语言下的可绘制资源。系统会根据设备当前的语言设置来加载相应的资源。

通过使用不同的drawable文件夹，可以为不同的屏幕密度、版本和方向提供适配的可绘制资源，以实现最佳的用户体验。

:::


