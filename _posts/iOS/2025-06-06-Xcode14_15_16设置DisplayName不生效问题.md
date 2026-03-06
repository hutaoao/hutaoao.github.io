---
title: Xcode14_15_16设置DisplayName不生效问题
date: 2025-06-06
description: Xcode14/15/16 设置Display Name不生效问题
tags: [iOS]
categories: [iOS]
---
# Xcode14/15/16 设置Display Name不生效问题

### 一、前言

早在Xcode13苹果就对Info.plist做了一次大改革，新建的OC项目默认Info.plist文件是“空的”，Swift项目甚至干脆连Info.plist文件都没有了，苹果这样做是为了建立一个新的Info.plist管理方式，把Info.plist物理文件中的配置挪到Xcode buildSetting中。

然而大部分开发者（比如我）并不买账，仍然使用旧的Info.plist文件模式，如何恢复到旧的Info.plist文件模式，新模式有什么弊端，更多内容见我之前的文章[《Xcode13 “消失”的Info.plist文件 》](https://juejin.cn/post/7086437261755203591)。

### 二、Xcode14 新变化

Xcode14 苹果又偷摸地改一刀。

如果你仍然使用旧的Info.plist文件模式，你会发现在Xcode面板（General - Display Name）设置App名字不会生效。如下图，我们把Display Name设置为“考拉”，你会发现App的名字仍然叫“Koala”（后面发现Info.plist中的Bundle display name 为 Koala）。

![1728647785630-ac95887e-f9e7-43d8-a3c0-3e4f3f783e41.png](/assets/img/posts/iOS/1728647785630-ac95887e-f9e7-43d8-a3c0-3e4f3f783e41-850366.png)

Xcode14之前，Xcode面板中的Display Name值和工程中的Info.plist物理文件CFBundleDisplayName字段同步。

从Xcode14开始，Xcode面板中的Display Name值不再和Info.plist物理文件CFBundleDisplayName字段同步，而是和 Build Setting - Info.plist Values（新增的模块）- Bundle Display Name的值同步（如下图）。

> 注意：这一点不受 Generate Info.plist File 开关（下文介绍）的影响，开关只影响Info.plist文件的生成模式。即 无论开关是YES还是NO，Xcode面板中的Display Name值都是取自Build Setting - Info.plist Values - Bundle Display Name。

![1728647942480-7793beca-2318-4b12-bbc7-58858193edf3.png](/assets/img/posts/iOS/1728647942480-7793beca-2318-4b12-bbc7-58858193edf3-801282.png)

#### 1. Generate Info.plist File字段

Build Settings - Generate Info.plist File（GENERATE\_INFOPLIST\_FILE）这个字段是Xcode13新引入的，它表示是否启用生成Info.plist文件的新模式，YES启动，NO不启用。新建的工程该字段默认为YES，旧的工程该值默认为NO。

![1728647998941-95d8f492-a222-46c4-b4be-ff7c66c10dd5.png](/assets/img/posts/iOS/1728647998941-95d8f492-a222-46c4-b4be-ff7c66c10dd5-492096.png)

当该值为YES时，Xcode使用新的Info.plist文件生成模式。Xcode会在打包时从 Build Setting - Info.plist Values模块 和 Info - Custom iOS Target Properties 中取数据生成最终的Info.plist文件。

当该值为NO时，Xcode使用旧的Info.plist文件生成模式。我们只需要像以前那样在工程中的Info.plist物理文件中配置参数，Xcode打包时会读取Info.plist物理文件中的配置生成最终包体里的Info.plist文件。

#### 2. Display Name不生效问题的原因

首先，我们先明确一点：App的名字最终是由编译出的包体里的Info.plist文件中`CFBundleDisplayName`字段决定的，如果Info.plist中没有`CFBundleDisplayName`字段则取`CFBundleName`字段的值。

由于Xcode14的新特性，Xcode面板中设置的 `Display Name`只会同步到 Build Setting - Info.plist Values - Bundle Display Name。

由于我们使用了旧的Info.plist文件模式（即 BuildSetting - Generate Info.plist File 的值为NO），导致 BuildSetting - Info.plist Values - Bundle Display Name 的设置不会同步到最终的生成的包体的Info.plist文件中。从而间接导致了在Xcode面板中设置Display Name不生效的问题。

### 三、解决方案

下面讨论的是 使用了旧的Info.plist文件生成模式（GENERATE\_INFOPLIST\_FILE=NO） 的情况，使用新的Info.plist文件生成模式理论上不会有这个问题。

#### 方案一（推荐）：

修改 Info.plist 文件中 `CFBundleDisplayName`（没有该字段则添加）的值为`$(INFOPLIST_KEY_CFBundleDisplayName)`

![1728648061892-62ce1d37-5ed0-42d3-ab52-089252c53543.png](/assets/img/posts/iOS/1728648061892-62ce1d37-5ed0-42d3-ab52-089252c53543-803021.png)

$(INFOPLIST\_KEY\_CFBundleDisplayName)为 Build Setting - Info.plist Values - Bundle Display Name值对应的环境变量（如下图）。

> Tips：如何获取 Build Settings 中字段对应的环境变量名？
>
> 选中某个字段，command+C复制，找个文本编辑器command+V粘贴出来，就可以看到了

![1728647942480-7793beca-2318-4b12-bbc7-58858193edf3.png](/assets/img/posts/iOS/1728647942480-7793beca-2318-4b12-bbc7-58858193edf3-492364.webp)

#### 方案二：

直接在 Info.plist 文件中修改 CFBundleDisplayName（没有该字段则添加）的值为你App的名字

![1728648165156-4dbcd49c-a2b9-4605-8d66-114172a973b1.png](/assets/img/posts/iOS/1728648165156-4dbcd49c-a2b9-4605-8d66-114172a973b1-234031.png)

该方案可以让App名字生效，但缺点是Xcode面板和Info.plist中不同步。例如 Info.plist中修改CFBundleDisplayName为“考拉”，App的名字生效了变成了“考拉”，但是Xcode面板中却是旧的值“ABCDE”。

#### 方案三：

使用新的Info.plist文件生成模式：设为YES即可（GENERATE\_INFOPLIST\_FILE=YES）


> 更新: 2024-10-11 20:07:03  
> 原文: <https://www.yuque.com/hutaoao/blog/zies7duxzzxom6gu>