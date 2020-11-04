---
title: centos服务器升级Git到最新版
date: 2017-05-12 01:02:23
tags: "Linux"
categories: "运维相关"
---

# 卸载旧版

一定要先卸载，不然虽然安装成功，但无法成功替换

```
# yum remove git
```

# 安装新版

命令行中的git-2.7.3需要替换为想要安装的版本

```
# cd /opt
# wget https://www.kernel.org/pub/software/scm/git/git-2.7.3.tar.gz
# tar -zxf git-2.7.3.tar.gz
```

<!-- more -->


编译安装新版本Git  

```
# yum install gcc perl-ExtUtils-MakeMaker
# yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel -y

# cd git-2.7.3
# ./configure --prefix=/usr/local
# make
# make install

# echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
# source /etc/bashrc
```

# 查看当前版本号  

```
git --version
```

# 总结  

至此，完结！
