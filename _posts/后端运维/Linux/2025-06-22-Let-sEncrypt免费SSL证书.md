---
title: Let-sEncrypt免费SSL证书
date: 2025-06-22
description: Let’s Encrypt 免费SSL证书
tags: [Linux]
categories: [后端运维, Linux]
---
# Let’s Encrypt 免费SSL证书

为了在您的网站上启用 HTTPS，您需要从证书颁发机构（CA）获取证书（一种文件）。Let’s Encrypt 是一个证书颁发机构（CA）。要从 Let’s Encrypt 获取您网站域名的证书，您必须证明您对域名的实际控制权。您可以在您的 Web 主机上运行使用 [ACME 协议](https://tools.ietf.org/html/rfc8555)的软件来获取 Let’s Encrypt 证书。



### [安装 snap](https://snapcraft.io/docs/installing-snap-on-centos)
```bash
# 在 CentOS 7 中添加 EPEL
$ sudo yum install epel-release

# 安装 snapd
$ sudo yum install snapd
$ systemctl enable --now snapd.socket
$ ln -s /var/lib/snapd/snap /snap
```

### 确保你snapd是最新的版本
```bash
$ sudo snap install core

$ sudo snap refresh core
```

### 安装Certbot
```bash
$ sudo snap install --classic certbot
```

### 建议软链接
```bash
$ sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

### 选择运行Certbot的方式
#### 1.直接获取并安装证书
运行此命令获取证书，并让Certbot自动编辑nginx配置以提供服务，只需一步即可打开HTTPS访问

```bash
$ sudo certbot --nginx
```

#### 2.仅获取证书
这时候需要手动更改nginx配置

```bash
$ sudo certbot certonly --nginx
```

> 中间可能提示找不到Nginx，此时需要设置Nginx软链
> ln -s /usr/local/webserver/nginx/sbin/nginx /usr/bin/nginx
> ln -s /usr/local/webserver/nginx/conf/ /etc/nginx
> 
> 验证下
> which nginx   》  /usr/bin/nginx



### 测试自动更新
```bash
$ sudo certbot renew --dry-run
```



> 注意： 一般国内 域名需备案后才能使用HTTPS



