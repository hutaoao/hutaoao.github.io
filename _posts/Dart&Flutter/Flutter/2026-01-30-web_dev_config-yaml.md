---
title: web_dev_config-yaml
date: 2026-01-30
description: webdevconfig.yaml
tags: [Dart&Flutter, Flutter, web]
categories: [Dart&Flutter, Flutter]
---
# web_dev_config.yaml

<font style="color:rgb(15, 17, 21);">Flutter 3.38 为 Web 端开发引入了一个非常实用的新功能：</font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">web_dev_config.yaml</font></code><font style="color:rgb(15, 17, 21);"> 配置文件。它让你能够以一种更标准、更持久的方式来管理开发服务器的各种设置，从而摆脱了以往需要记忆和输入一长串命令行参数的日子。</font>

<font style="color:rgb(15, 17, 21);">下面这份文档将带你全面了解这个配置文件，助你高效上手。</font>

### <font style="color:rgb(15, 17, 21);">⚙️</font><font style="color:rgb(15, 17, 21);"> 配置文件详解与基础用法</font>

#### <font style="color:rgb(15, 17, 21);">文件位置与结构</font>

<code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">web_dev_config.yaml</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">需要放置在您 Flutter 项目的</font>**<font style="color:rgb(15, 17, 21);">根目录</font>**<font style="color:rgb(15, 17, 21);">下（与</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">pubspec.yaml</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">同级）。它的结构清晰，所有配置都位于顶级的</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">server</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">键下。</font>

#### <font style="color:rgb(15, 17, 21);">核心配置项</font>

| <font style="color:rgb(15, 17, 21);">配置分类</font> | <font style="color:rgb(15, 17, 21);">配置项</font> | <font style="color:rgb(15, 17, 21);">类型</font> | <font style="color:rgb(15, 17, 21);">默认值</font> | <font style="color:rgb(15, 17, 21);">描述</font> |
| --- | --- | --- | --- | --- |
| **<font style="color:rgb(15, 17, 21);">服务器绑定</font>** | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">host</font></code> | <font style="color:rgb(15, 17, 21);">字符串</font> | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">"localhost"</font></code> | <font style="color:rgb(15, 17, 21);">开发服务器绑定的主机名或IP地址。使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">"0.0.0.0"</font></code><br/><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">允许局域网设备访问</font><font style="color:rgb(15, 17, 21);">。</font> |
| | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">port</font></code> | <font style="color:rgb(15, 17, 21);">整数</font> | <font style="color:rgb(15, 17, 21);">随机端口</font> | <font style="color:rgb(15, 17, 21);">指定开发服务器使用的固定端口</font><font style="color:rgb(15, 17, 21);">。</font> |
| **<font style="color:rgb(15, 17, 21);">HTTPS安全</font>** | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">https</font></code> | <font style="color:rgb(15, 17, 21);">映射</font> | <font style="color:rgb(15, 17, 21);">-</font> | <font style="color:rgb(15, 17, 21);">配置TLS证书以启用HTTPS开发服务器</font><font style="color:rgb(15, 17, 21);">。</font> |
| | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">→ cert-path</font></code> | <font style="color:rgb(15, 17, 21);">字符串</font> | <font style="color:rgb(15, 17, 21);">-</font> | <font style="color:rgb(15, 17, 21);">TLS证书文件（如</font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">.pem</font></code><br/><font style="color:rgb(15, 17, 21);">）的路径</font><font style="color:rgb(15, 17, 21);">。</font> |
| | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">→ cert-key-path</font></code> | <font style="color:rgb(15, 17, 21);">字符串</font> | <font style="color:rgb(15, 17, 21);">-</font> | <font style="color:rgb(15, 17, 21);">TLS证书私钥文件路径</font><font style="color:rgb(15, 17, 21);">。</font> |
| **<font style="color:rgb(15, 17, 21);">请求代理</font>** | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">proxy</font></code> | <font style="color:rgb(15, 17, 21);">列表</font> | <font style="color:rgb(15, 17, 21);">-</font> | <font style="color:rgb(15, 17, 21);">定义代理规则，将特定请求转发至后端服务器</font><font style="color:rgb(15, 17, 21);">。</font> |
| | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">→ target</font></code> | <font style="color:rgb(15, 17, 21);">字符串</font> | **<font style="color:rgb(15, 17, 21);">必填</font>** | <font style="color:rgb(15, 17, 21);">后端服务的基地址（如</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">http://localhost:5000/</font></code><br/><font style="color:rgb(15, 17, 21);">）</font><font style="color:rgb(15, 17, 21);">。</font> |
| | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">→ prefix</font></code> | <font style="color:rgb(15, 17, 21);">字符串</font> | **<font style="color:rgb(15, 17, 21);">必填</font>** | <font style="color:rgb(15, 17, 21);">匹配请求路径的前缀（如</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/api/</font></code><br/><font style="color:rgb(15, 17, 21);">）</font><font style="color:rgb(15, 17, 21);">。</font> |
| | <code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">→ replace</font></code> | <font style="color:rgb(15, 17, 21);">字符串</font> | <font style="color:rgb(15, 17, 21);">可选</font> | <font style="color:rgb(15, 17, 21);">在转发时替换路径中的</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">prefix</font></code><br/><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">部分</font><font style="color:rgb(15, 17, 21);">。</font> |

#### <font style="color:rgb(15, 17, 21);">基础配置示例</font>

<font style="color:rgb(15, 17, 21);">一个基础的配置文件，用于配置服务器和启用HTTPS，看起来是这样的：</font>

<font style="color:rgb(15, 17, 21);">yaml</font>

```yaml
# web_dev_config.yaml
server:
  host: "0.0.0.0"    # 允许局域网访问
  port: 8080          # 固定端口
  https:
    cert-path: "/path/to/your/certificate.pem"      # 替换为你的证书路径
    cert-key-path: "/path/to/your/private_key.pem"  # 替换为你的私钥路径
```

### <font style="color:rgb(15, 17, 21);">🔌</font><font style="color:rgb(15, 17, 21);"> 核心功能：请求代理详解</font>

<font style="color:rgb(15, 17, 21);">这是解决开发阶段跨域问题（CORS）的利器。你可以通过配置代理规则，将特定API请求无缝转发到后端开发服务器。</font>

#### <font style="color:rgb(15, 17, 21);">代理规则示例</font>

<font style="color:rgb(15, 17, 21);">假设你的后端API运行在</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">http://localhost:5000</font></code><font style="color:rgb(15, 17, 21);">，并且所有接口都以</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/api</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">开头，可以这样配置：</font>

<font style="color:rgb(15, 17, 21);">yaml</font>

```yaml
server:
  proxy:
    - target: "http://localhost:5000/"
      prefix: "/api/"
```

<font style="color:rgb(15, 17, 21);">配置后，你在Flutter应用中发起的请求</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">http://localhost:8080/api/users</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">会被开发服务器自动代理到</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">http://localhost:5000/api/users</font></code><font style="color:rgb(15, 17, 21);">。</font>

#### <font style="color:rgb(15, 17, 21);">路径重写</font>

<font style="color:rgb(15, 17, 21);">如果你的后端路径结构不同，可以使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">replace</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">选项进行重写。例如，将请求到</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/auth/</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">的路径代理到后端并替换为</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">/oauth/</font></code><font style="color:rgb(15, 17, 21);">：</font>

<font style="color:rgb(15, 17, 21);">yaml</font>

```yaml
server:
  proxy:
    - target: "http://localhost:3000/"
      prefix: "/auth/"
      replace: "/oauth/"  # 请求 /auth/login 将被代理为 http://localhost:3000/oauth/login
```

* <code>**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">prefix</font>**</code><font style="color:rgb(15, 17, 21);">：定义要匹配的请求路径前缀。</font>
* <code>**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">replace</font>**</code><font style="color:rgb(15, 17, 21);">：定义在转发时替换</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">prefix</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">部分的字符串。如果未设置，则使用原始路径。</font>

### <font style="color:rgb(15, 17, 21);">🚀</font><font style="color:rgb(15, 17, 21);"> 如何使用</font>

1. **<font style="color:rgb(15, 17, 21);">创建文件</font>**<font style="color:rgb(15, 17, 21);">：在项目根目录创建</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">web_dev_config.yaml</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">文件。</font>
2. **<font style="color:rgb(15, 17, 21);">编写配置</font>**<font style="color:rgb(15, 17, 21);">：根据你的需求，填入上述配置。</font>
3. **<font style="color:rgb(15, 17, 21);">启动项目</font>**<font style="color:rgb(15, 17, 21);">：像往常一样使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">flutter run -d web-server</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">命令启动Web应用。Flutter会自动读取并使用你的配置文件。</font>

<font style="color:rgb(15, 17, 21);">启动后，控制台输出的地址会反映你的配置，例如：</font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">Launching lib/main.dart on Web Server in debug mode... http://0.0.0.0:8080/</font></code><font style="color:rgb(15, 17, 21);">。</font>

### <font style="color:rgb(15, 17, 21);">💎</font><font style="color:rgb(15, 17, 21);"> 最佳实践与注意事项</font>

* **<font style="color:rgb(15, 17, 21);">环境隔离</font>**<font style="color:rgb(15, 17, 21);">：请记住，</font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">web_dev_config.yaml</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">中的配置</font>**<font style="color:rgb(15, 17, 21);">仅在使用</font>\*\*\*\*<font style="color:rgb(15, 17, 21);"> </font>**<code>**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">flutter run -d web-server</font>**</code>**<font style="color:rgb(15, 17, 21);"> </font>\*\*\*\*<font style="color:rgb(15, 17, 21);">时生效</font>**<font style="color:rgb(15, 17, 21);">，不会影响</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">flutter build web</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">构建的生产版本</font><font style="color:rgb(15, 17, 21);">。生产环境的代理仍需在Nginx等服务器软件中配置。</font>
* **<font style="color:rgb(15, 17, 21);">团队协作</font>**<font style="color:rgb(15, 17, 21);">：建议将此文件纳入版本控制，这样就能让团队所有成员共享一套统一的开发环境配置。</font>
* **<font style="color:rgb(15, 17, 21);">安全提醒</font>**<font style="color:rgb(15, 17, 21);">：配置文件中的HTTPS私钥路径可能会暴露你的证书信息。请确保不将真实的私钥文件或包含敏感路径的配置文件提交到公共代码库。</font>
* **<font style="color:rgb(15, 17, 21);">功能定位</font>**<font style="color:rgb(15, 17, 21);">：此配置主要服务于</font>**<font style="color:rgb(15, 17, 21);">开发阶段</font>**<font style="color:rgb(15, 17, 21);">。它的代理功能是为了方便你与本地的后端服务联调，解决的是开发时的跨域问题，并非用于生产环境。</font>


> 更新: 2025-11-13 09:52:10  
> 原文: <https://www.yuque.com/hutaoao/blog/kve9gsumn5lg0bk8>