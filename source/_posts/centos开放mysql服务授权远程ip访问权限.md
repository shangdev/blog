---
title: centos开放mysql服务授权远程ip访问权限
date: 2017-05-12 01:03:27
tags: "Linux"
categories: "运维相关"
---

本篇讲如何放开 mysql 的远程访问权限。

<!-- more -->

> 针对iptables设置

# 查看  

查看当前iptables状态  
```
sudo /etc/init.d/iptables status
```

# 设置  

```
# 打开iptables设置文件
sudo vi /etc/sysconfig/iptables

# 删除原3306规则DROP，新增3306规则ACCEPT
# ip：需要开放的ip地址
# 此处需要注意，-A INPUT -p tcp -m tcp --dport 3306 -j DROP这条规则时放到新增规则之后的，不然由于优先级无法生效。
-A INPUT -s ip  -p tcp --dport 3306 -j ACCEPT
```

继续查看iptables状态
```
sudo /etc/init.d/iptables status
```
mysql设置  

```
# 展示用户(root)以及地址权限
SHOW GRANTS FOR 'root'@'localhost'

# 参数说明
# -- user 所属用户
# -- password 当前用户登陆密码
# -- ip 将要授权的远程ip，如果是对所有ip开发，则替换为 %
GRANT ALL PRIVILEGES ON *.* TO 'user'@'ip' IDENTIFIED BY 'password' WITH GRANT OPTION
```
查看  

```
# 刷新立即生效
flush privileges;
```
