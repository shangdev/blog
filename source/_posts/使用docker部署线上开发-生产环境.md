---
title: 使用Docker部署线上开发/生产环境
date: 2018-09-02 01:01:48
tags: "Docker"
category: "运维相关"
---

# 写在前面

不擅长运维又很喜欢运维的我，每次在线上或者线下部署开发/生产环境时都无聊到要死，感觉就跟ctrl+c、ctrl+v似的，既浪费了大把的时间，又觉得索然无味，所以就有了这篇文章。

Docker是什么？借用wikipedia上的解释：

> Docker是一个开放源代码软件项目，让应用程序布署在软件容器下的工作可以自动化进行，借此在Linux操作系统上，提供一个额外的软件抽象层，以及操作系统层虚拟化的自动管理机制。

至于我们为什么要使用Docker？那就得说到它的众多优势了：

- 更高效的利用系统资源
- 更快的启动时间
- 一致的运行环境
- 持续交付和部署
- 更轻松的迁移
- 更轻松的维护和扩展

<!-- more -->

# 安装Docker

Docker有两个版本，CE（社区版、免费）和EE（企业版、付费），这里我使用到的是社区版，主要讲在Linux（CentOS 7）下到安装：

> 注意：这里讲到的安装前提是以前没有安装过Docker。

## 系统要求

- CentOS 7 64位及以上
- 内核 >= 3.10

## 使用yum安装

首先安装以下依赖包：

```
# sudo yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2
```

添加yum安装源，任选其一，国内源会快些：

```
# sudo yum-config-manager \
    --add-repo \
    https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo

官方源
# sudo yum-config-manager \
#     --add-repo \
#     https://download.docker.com/linux/centos/docker-ce.repo
```

安装Docker CE

```
# sudo yum makecache fast
# sudo yum install docker-ce
```

启用Docker CE：

```
# sudo systemctl enable docker
# sudo systemctl start docker
```

默认情况下，只有root和docker用户组才能访问Docker引擎，因此需要将使用Docker的用户加入到Docker用户组中。

建立docker组：

```
# sudo groupadd docker
```

添加当前用户至Docker组：

```
# sudo usermod -aG docker $USER
```

查看是否安装：

```
# docker info
```

同时我们还需要安装Docker-compose：

```
注意：其中1.22.0是版本号，将其替换为最新的
# sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
# sudo chmod +x /usr/local/bin/docker-compose
```

# 使用Docker

## 镜像

拉取镜像：

```
# docker pull [仓库名]:[标签]
```
例如，拉取版本为16.04的Ubuntu：

```
# docker pull ubuntu:16.04
```

列出本机所有的image（镜像）：

```
# docker image ls
```

删除本机指定镜像，其中，[镜像]值可以是镜像短ID、长ID、镜像名、镜像摘要：

```
# docker image rm [镜像]
```

## 容器

容器轻量到我们可以随时删除和启用它。

新建及启用：

```
# docker run [镜像名]
```

可能需要进入容器bash终端：

```
# docker run -t -i [镜像名] /bin/bash
```

查看当前运行的所有容器：

```
# docker container ls
```

终止一个容器，[容器名]值可以是镜像短ID、长ID、镜像名、镜像摘要：

```
# docker stop [容器名]
```

重启一个容器：

```
# docker restart [容器名]
```

删除指定的容器，需要先终止：

```
# docker container rm [容器名]
```

# 最好的实践

> [khs1994-docker/lnmp](https://github.com/khs1994-docker/lnmp)是基于Docker Compose的一键搭建 LNMP(LEMP) 开发环境和生产环境(集群)的开源项目。

该项目严禁直接编辑除 .env 文件以外的所有文件，目的是方便我们可以 **无痛升级** 。虽然如此，但是仍然可以实现全部的自定义功能，[在这里](https://github.com/khs1994-docker/lnmp/blob/master/docs/config.md)查看指南。

## 先决条件

- Docker CE 18.06 Stable +
- Docker Compose 1.22.0+
- WSL (Windows Only)

## 快速指南

拉取项目到你想存放的指定(如：/var/www/example/)目录：

```
# git clone --depth=1 --recursive https://code.aliyun.com/khs1994-docker/lnmp.git /var/www/example/
```

进入项目并启用一个Demo：

```
# cd /var/www/example/
# ./lnmp-docker up
```

启用后访问IP可得到如下信息即可正常运行：
Welcome use khs1994-docker/lnmp v18.10 x86_64 With Pull Docker Image development

## 数据库切换

编辑 .env 文件，修改变量 LNMP_MYSQL_VERSION 的值，这里我们把默认的版本降级为 5.6.41：

```
LNMP_MYSQL_VERSION=5.6.41
```

注意，先前我们运行最新版的mysql，现在我们降级到 5.6.41 版本，导致数据不兼容，所以mysql容器没起来，所以需要先删除mysql数据卷，再启动：

```
# ./lnmp-docker down -v
# docker volume rm lnmp_mysql-data
# ./lnmp-docker up
```

而由于 mysql5.6.41 版本中不支持变量log_timestamps（5.7中新增），所以我们需要把该项目中该变量的定义注释掉：

```
# vi lnmp/config/mysql/docker.cnf
```

注释其中的 log_timestamps=SYSTEM

## SSl证书

该项目集成了acme.sh，[在这里](https://github.com/khs1994-docker/lnmp/blob/master/docs/nginx/issue-ssl.md)查看

> 提示，`*.example.com` 的证书不支持 `example.com` 所以一个主域要写两次

> 提示，第一个网址不用加 `-d` 参数，后面的需要加 `-d` 参数

```
# ./lnmp-docker ssl example.cn -d *.example.cn
```

# 总结

这是第一次接触 Docker，不明之处还有很多，所以会持续学习并更新该文，谢谢大家的阅读！