---
title: App上传审核应用时出现包含bitcode的报错
date: 2025-06-02
description: App 上传审核应用时出现包含 bitcode 的报错
tags: [iOS]
categories: [iOS]
---
# App 上传审核应用时出现包含 bitcode 的报错

## 问题描述

在 Xcode 开发环境下，将采用网易云信相关产品适配的 iOS 客户端 SDK（例如 [NERtcSDK](https://doc.yunxin.163.com/nertc/concept?platform=client) 或 [NIMSDK](https://doc.yunxin.163.com/messaging2/concept?platform=client)）开发的应用打包提交到苹果应用商店（App Store）审核时，出现包含 Bitcode 的报错，诸如如下报错：

```plain
shInvalid Executable. The executable 'XXX.app/Frameworks/NERtcSDK.framework

NERtcSDK' contains bitcode.
```

其中，Xcode16 报错情况较多。

## 问题原因

Bitcode 是一种中间表示形式，在 Xcode 中打包提交到 App Store 审核时，如果出现包含 Bitcode 的报错，这通常意味着您的应用没有正确包含 Bitcode。Bitcode 是苹果的一项要求，它允许苹果在 App Store 中对您的应用进行进一步的优化。

当提交应用到 App Store 时出现与 Bitcode 相关的问题，您需要手动移除 framework 中的 Bitcode。framework 是指 macOS 和 iOS 项目中的一个软件框架，它是一种包含代码、资源和其他文件的包，用于实现特定的功能或服务。framework 通常用于提供应用程序的某些部分，如用户界面元素、数据处理功能或其他服务。

## 解决方法

### 工具介绍

<code>xcrun bitcode_strip</code> 是一个命令行工具，用于手动去除对应 framework 的 Bitcode，命令格式如下：

```plain
shxcrun bitcode_strip -r ${framework_path} -o ${framework_path}
```

<code>${framework_path}</code> 是一个占位符，表示 framework 的二进制文件路径。在实际使用命令时，您需要将 <code>${framework_path}</code> 替换为具体的文件路径。

### 使用示例

假设您有一个名为 <code>NIMSDK.framework</code> 的 framework，并且它位于 /path/to/~/NIMSDK.framework 路径，那么您可以按照以下方式处理：

1. 通过 <code>cd</code> 命令进入到 <code>NIMSDK.framework</code> 的路径。

如果是通过 <code>pod install</code> 获取的 SDK，则进入 <code>pods</code> 文件夹。

2. 执行以下命令检查 framework 是否包含 bitcode，返回 <code>0</code> 即为不包含。

```plain
shotool -l NIMSDK | grep __LLVM | wc -l
```

3. 如果检测结果不是 <code>0</code>，则继续执行以下命令移除 <code>NIMSDK.framework</code> 的 Bitcode。

```plain
shxcrun bitcode_strip -r NIMSDK -o NIMSDK
```

![1730339559764-ef4138fa-bcb0-475d-8b9c-b4017fe31059.png](/assets/img/posts/iOS/1730339559764-ef4138fa-bcb0-475d-8b9c-b4017fe31059-833688.png)


