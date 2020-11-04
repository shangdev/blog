---
title: Let’s Encrypt 证书指南
date: 2019-02-01 00:21:48
tags:
- https
categories:
- 运维相关
---

现在，Letsencrypt 支持通配符证书申请了。

# 证书申请

首先，获取 [Certbot](https://certbot.eff.org/) 客户端：

```sh
$ wget https://dl.eff.org/certbot-auto
$ chmod a+x certbot-auto
```

<!-- more -->

接着，使用 Certbot 客户端申请证书，使用 dns-01 方式，替换掉下方 *.xxx.com 为待申请证书的域名。

需要注意的是，如果申请单域名证书，直接替换为要申请证书的域名；如果申请的是泛域名证书，替换格式为 “*.xxx.com”。

```sh
$ ./certbot-auto certonly  -d "*.xxx.com" --manual --preferred-challenges dns-01  --server https://acme-v02.api.letsencrypt.org/directory
```

根据命令行的提示，输入对应的信息，在 “Please deploy a DNS TXT record under the name ...” 这一步暂停一下，到域名解析那里添加提示的 TXT 记录。

检查本地检查记录是否已生效：

```sh
$ dig -t txt _acme-challenge.xxx.com @8.8.8.8
```

如果解析已生效，继续回到命令行，按回车键继续。不出意外，在短暂的验证后，会出现 Congratulations 的字样，表明证书申请成果。

生成的证书存放目录结构如下：

```sh
$ tree /etc/letsencrypt/live/xxx.com/
├── cert.pem
├── chain.pem
├── fullchain.pem
└── privkey.pem
```

# 证书续期

Let's encrypt 证书有效期为90天，到期前会自动邮件（邮件为申请证书时填写的邮件）提示进行续期，续期命令：

```sh
$ certbot-auto renew
```

需要注意的是，通过 dns-01 方式，上面这条命令会报错。所以，我们直接执行初始的申请命令，即：

```sh
$ ./certbot-auto certonly  -d "*.xxx.com" --manual --preferred-challenges dns-01  --server https://acme-v02.api.letsencrypt.org/directory
```

这样，新的证书就申请下来了。

# Nginx 配置

```sh
$ server {
$   listen 443 ssl;
$   server_name *.xxx.com;
$
$   ssl on;
$   ssl_certificate /etc/cert/xxx.com/fullchain.pem;
$   ssl_certificate_key /etc/cert/xxx.com/privkey.pem;
$   ssl_trusted_certificate  /etc/cert/xxx.com/chain.pem;
$
$   ...
$
$ }
```