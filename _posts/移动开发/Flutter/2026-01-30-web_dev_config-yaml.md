---
title: web_dev_config-yaml
date: 2026-01-30
description: webdevconfig.yaml
tags: [移动开发, Flutter, Flutter, web]
categories: [移动开发, Flutter]
---
# web_dev_config.yaml

Flutter 3.38 为 Web 端开发引入了一个非常实用的新功能：<code>web_dev_config.yaml</code> 配置文件。它让你能够以一种更标准、更持久的方式来管理开发服务器的各种设置，从而摆脱了以往需要记忆和输入一长串命令行参数的日子。

下面这份文档将带你全面了解这个配置文件，助你高效上手。

### ⚙️ 配置文件详解与基础用法

#### 文件位置与结构

<code>web_dev_config.yaml</code> 需要放置在您 Flutter 项目的**根目录**下（与 <code>pubspec.yaml</code> 同级）。它的结构清晰，所有配置都位于顶级的 <code>server</code> 键下。

#### 核心配置项

| 配置分类 | 配置项 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| **服务器绑定** | <code>host</code> | 字符串 | <code>"localhost"</code> | 开发服务器绑定的主机名或IP地址。使用 <code>"0.0.0.0"</code><br/> 允许局域网设备访问。 |
| | <code>port</code> | 整数 | 随机端口 | 指定开发服务器使用的固定端口。 |
| **HTTPS安全** | <code>https</code> | 映射 | - | 配置TLS证书以启用HTTPS开发服务器。 |
| | <code>→ cert-path</code> | 字符串 | - | TLS证书文件（如<code>.pem</code><br/>）的路径。 |
| | <code>→ cert-key-path</code> | 字符串 | - | TLS证书私钥文件路径。 |
| **请求代理** | <code>proxy</code> | 列表 | - | 定义代理规则，将特定请求转发至后端服务器。 |
| | <code>→ target</code> | 字符串 | **必填** | 后端服务的基地址（如 <code>http://localhost:5000/</code><br/>）。 |
| | <code>→ prefix</code> | 字符串 | **必填** | 匹配请求路径的前缀（如 <code>/api/</code><br/>）。 |
| | <code>→ replace</code> | 字符串 | 可选 | 在转发时替换路径中的 <code>prefix</code><br/> 部分。 |

#### 基础配置示例

一个基础的配置文件，用于配置服务器和启用HTTPS，看起来是这样的：

```yaml
# web_dev_config.yaml
server:
  host: "0.0.0.0"    # 允许局域网访问
  port: 8080          # 固定端口
  https:
    cert-path: "/path/to/your/certificate.pem"      # 替换为你的证书路径
    cert-key-path: "/path/to/your/private_key.pem"  # 替换为你的私钥路径
```

### 🔌 核心功能：请求代理详解

这是解决开发阶段跨域问题（CORS）的利器。你可以通过配置代理规则，将特定API请求无缝转发到后端开发服务器。

#### 代理规则示例

假设你的后端API运行在 <code>http://localhost:5000</code>，并且所有接口都以 <code>/api</code> 开头，可以这样配置：

```yaml
server:
  proxy:
    - target: "http://localhost:5000/"
      prefix: "/api/"
```

配置后，你在Flutter应用中发起的请求 <code>http://localhost:8080/api/users</code> 会被开发服务器自动代理到 <code>http://localhost:5000/api/users</code>。

#### 路径重写

如果你的后端路径结构不同，可以使用 <code>replace</code> 选项进行重写。例如，将请求到 <code>/auth/</code> 的路径代理到后端并替换为 <code>/oauth/</code>：

```yaml
server:
  proxy:
    - target: "http://localhost:3000/"
      prefix: "/auth/"
      replace: "/oauth/"  # 请求 /auth/login 将被代理为 http://localhost:3000/oauth/login
```

* <code>**prefix**</code>：定义要匹配的请求路径前缀。
* <code>**replace**</code>：定义在转发时替换 <code>prefix</code> 部分的字符串。如果未设置，则使用原始路径。

### 🚀 如何使用

1. **创建文件**：在项目根目录创建 <code>web_dev_config.yaml</code> 文件。
2. **编写配置**：根据你的需求，填入上述配置。
3. **启动项目**：像往常一样使用 <code>flutter run -d web-server</code> 命令启动Web应用。Flutter会自动读取并使用你的配置文件。

启动后，控制台输出的地址会反映你的配置，例如：<code>Launching lib/main.dart on Web Server in debug mode... http://0.0.0.0:8080/</code>。

### 💎 最佳实践与注意事项

* **环境隔离**：请记住，<code>web_dev_config.yaml</code> 中的配置**仅在使用\*\*\*\* **<code>**flutter run -d web-server**</code>** \*\*\*\*时生效**，不会影响 <code>flutter build web</code> 构建的生产版本。生产环境的代理仍需在Nginx等服务器软件中配置。
* **团队协作**：建议将此文件纳入版本控制，这样就能让团队所有成员共享一套统一的开发环境配置。
* **安全提醒**：配置文件中的HTTPS私钥路径可能会暴露你的证书信息。请确保不将真实的私钥文件或包含敏感路径的配置文件提交到公共代码库。
* **功能定位**：此配置主要服务于**开发阶段**。它的代理功能是为了方便你与本地的后端服务联调，解决的是开发时的跨域问题，并非用于生产环境。


