---
title: HarmonyOSNEXT应用上架流程
date: 2025-12-20
description: HarmonyOS NEXT应用上架流程
tags: [HarmonyOS, harmonyos]
categories: [HarmonyOS]
---
# HarmonyOS NEXT应用上架流程

![1736734764939-42143fb1-2ac5-45b2-9e33-515a6c350888.png](/assets/img/posts/harmonyos/1736734764939-42143fb1-2ac5-45b2-9e33-515a6c350888-257284.png)

### 生成密钥和证书请求文件（p12）

在主菜单栏单击Build > Generate Key and CSR

在Key Store File中，单击New进行创建

在Create Key Store窗口中，填写密钥库信息后，单击OK。

* Key Store File：设置密钥库文件存储路径，并填写p12文件名。
* Password：设置密钥库密码，必须由大写字母、小写字母、数字和特殊符号中的两种以上字符的组合，长度至少为8位。请记住该密码，后续签名配置需要使用。
* Confirm Password：再次输入密钥库密码。

![1736734893118-d1baeb99-074a-4795-a207-392128975175.png](/assets/img/posts/harmonyos/1736734893118-d1baeb99-074a-4795-a207-392128975175-789747.png)

![1736734915046-5c0ec4d3-7af1-4bed-a2d3-90fd45a8b0ae.png](/assets/img/posts/harmonyos/1736734915046-5c0ec4d3-7af1-4bed-a2d3-90fd45a8b0ae-655807.png)

在Generate Key and CSR界面中，继续填写密钥信息后，单击Next。

* Alias：密钥的别名信息，用于标识密钥名称。请记住该别名，后续签名配置需要使用。
* Password：密钥对应的密码，与密钥库密码保持一致，无需手动输入。

![1736734989563-aece6040-4ca2-4ceb-88c3-261e81b75f92.png](/assets/img/posts/harmonyos/1736734989563-aece6040-4ca2-4ceb-88c3-261e81b75f92-679849.png)

在Generate Key and CSR界面，设置CSR文件存储路径和CSR文件名。![1736735022463-b5457ad0-28d4-48be-a308-232bb5857e6d.png](/assets/img/posts/harmonyos/1736735022463-b5457ad0-28d4-48be-a308-232bb5857e6d-241512.png)

单击OK按钮，创建CSR文件成功，可以在存储路径下获取生成的密钥库文件（.p12）和证书请求文件（.csr）。

![1736735047270-71e153fb-4540-4166-a5ca-22bc23bb4a56.png](/assets/img/posts/harmonyos/1736735047270-71e153fb-4540-4166-a5ca-22bc23bb4a56-182405.png)

### 申请发布证书（.cer）

登录AppGallery Connect，选择“证书、APP ID和Profile”\ 在左侧导航栏选择“证书、APP ID和Profile > 证书”，进入“证书”页面，点击“新增证书”\
![1736735317806-6a14788a-8e84-4e82-bfb6-a8c755407d8f.png](/assets/img/posts/harmonyos/1736735317806-6a14788a-8e84-4e82-bfb6-a8c755407d8f-691562.png)



### 申请发布Profile

1. 登录[AppGallery Connect](https://developer.huawei.com/consumer/cn/service/josp/agc/index.html)，选择“我的项目”。
2. 找到对应项目，点击项目卡片中需要发布的元服务。
3. 导航选择“HarmonyOS应用 > HAP Provision Profile管理”，进入“管理HAP Provision Profile”页面，点击“添加”。

![1736735397421-9dd3746a-9b90-45bb-aba0-8d6627f843f6.png](/assets/img/posts/harmonyos/1736735397421-9dd3746a-9b90-45bb-aba0-8d6627f843f6-572930.png)



> 注册调试设备（真机）
> <https://developer.huawei.com/consumer/cn/doc/app/agc-help-add-device-0000001946142249#section67331926102911>
> 获取UUID：
> <code>/Volumes/T7/DevEco-Studio.app/Contents/sdk/default/openharmony/toolchains/hdc shell bm get --udid</code>

### 配置项目

![1736735157972-db08429c-1d3a-410e-bbd2-edc0f072567d.png](/assets/img/posts/harmonyos/1736735157972-db08429c-1d3a-410e-bbd2-edc0f072567d-471167.png)

### 使用真机调试

<https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-run-device-V5>

### 打生产包

![1736735440240-39247552-2271-4524-921e-676340bd3aed.png](/assets/img/posts/harmonyos/1736735440240-39247552-2271-4524-921e-676340bd3aed-618416.png)

> 真机调试
> <https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-run-device-V5>

### 软件包上传

![1736735524993-990892eb-ef77-4935-a895-77701f00d057.png](/assets/img/posts/harmonyos/1736735524993-990892eb-ef77-4935-a895-77701f00d057-585535.png)


