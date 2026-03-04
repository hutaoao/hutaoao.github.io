---
title: HarmonyOS NEXT 应用上架流程
date: 2025-03-04
description: 详细介绍HarmonyOS NEXT应用上架的完整流程,包括生成密钥、申请证书、配置项目、打包上传等
tags: [HarmonyOS, 应用上架, AppGallery Connect]
categories: [HarmonyOS]
---

![上架流程概览](/assets/img/posts/harmonyos/app-publish/1736734764939-42143fb1-2ac5-45b2-9e33-515a6c350888.png)

## 生成密钥和证书请求文件(p12)

在主菜单栏单击 **Build > Generate Key and CSR**

在 Key Store File 中,单击 New 进行创建

在 Create Key Store 窗口中,填写密钥库信息后,单击 OK。

- **Key Store File**: 设置密钥库文件存储路径,并填写p12文件名。
- **Password**: 设置密钥库密码,必须由大写字母、小写字母、数字和特殊符号中的两种以上字符的组合,长度至少为8位。请记住该密码,后续签名配置需要使用。
- **Confirm Password**: 再次输入密钥库密码。

![创建Key Store](/assets/img/posts/harmonyos/app-publish/1736734893118-d1baeb99-074a-4795-a207-392128975175.png)

在 Generate Key and CSR 界面中,继续填写密钥信息后,单击 Next。

- **Alias**: 密钥的别名信息,用于标识密钥名称。请记住该别名,后续签名配置需要使用。
- **Password**: 密钥对应的密码,与密钥库密码保持一致,无需手动输入。

![填写密钥信息](/assets/img/posts/harmonyos/app-publish/1736734915046-5c0ec4d3-7af1-4bed-a2d3-90fd45a8b0ae.png)

在 Generate Key and CSR 界面,设置 CSR 文件存储路径和 CSR 文件名。

![设置CSR文件](/assets/img/posts/harmonyos/app-publish/1736734989563-aece6040-4ca2-4ceb-88c3-261e81b75f92.png)

单击 OK 按钮,创建 CSR 文件成功,可以在存储路径下获取生成的密钥库文件(.p12)和证书请求文件(.csr)。

![文件创建成功](/assets/img/posts/harmonyos/app-publish/1736735022463-b5457ad0-28d4-48be-a308-232bb5857e6d.png)

## 申请发布证书(.cer)

登录 AppGallery Connect,选择"证书、APP ID和Profile"

在左侧导航栏选择"证书、APP ID和Profile > 证书",进入"证书"页面,点击"新增证书"

![新增证书](/assets/img/posts/harmonyos/app-publish/1736735317806-6a14788a-8e84-4e82-bfb6-a8c755407d8f.png)

## 申请发布 Profile

1. 登录[AppGallery Connect](https://developer.huawei.com/consumer/cn/service/josp/agc/index.html),选择"我的项目"。
2. 找到对应项目,点击项目卡片中需要发布的元服务。
3. 导航选择"HarmonyOS应用 > HAP Provision Profile管理",进入"管理HAP Provision Profile"页面,点击"添加"。

![添加Profile](/assets/img/posts/harmonyos/app-publish/1736735397421-9dd3746a-9b90-45bb-aba0-8d6627f843f6.png)

### 注册调试设备(真机)

> 注册调试设备(真机)
>
> [https://developer.huawei.com/consumer/cn/doc/app/agc-help-add-device-0000001946142249#section67331926102911](https://developer.huawei.com/consumer/cn/doc/app/agc-help-add-device-0000001946142249#section67331926102911)

> 获取UUID:
>
> `/Volumes/T7/DevEco-Studio.app/Contents/sdk/default/openharmony/toolchains/hdc shell bm get --udid`

## 配置项目

![配置项目](/assets/img/posts/harmonyos/app-publish/1736735157972-db08429c-1d3a-410e-bbd2-edc0f072567d.png)

## 使用真机调试

[https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-run-device-V5](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-run-device-V5)

## 打生产包

![打生产包](/assets/img/posts/harmonyos/app-publish/1736735440240-39247552-2271-4524-921e-676340bd3aed.png)

> 真机调试
>
> [https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-run-device-V5](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-run-device-V5)

## 软件包上传

![软件包上传](/assets/img/posts/harmonyos/app-publish/1736735524993-990892eb-ef77-4935-a895-77701f00d057.png)
