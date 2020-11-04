---
title: Ubuntu14.04部署LNMP环境小记
date: 2017-04-18 23:57:12
tags: "Linux"
categories: "运维相关"
---
# 背景  

> 话不多说，就是在Ubuntu14.04上部署LNMP开发环境  

# 需要安装的环境    
   -  nginx
   -  php5.5；
   -  php常用扩展;  
   -  php-fpm;  
   -  mysql5.6；
   -  Git1.9.1;

<!-- more -->

# 开工  

> 以下所有命令都是在root用户下执行  

## 组件升级  
```
apt-get update;
```
## nginx安装  
```
apt-get install nginx
```

## php5.5安装  

```
apt-get install php5

# 查看php5安装结果
php -v
```
## php5常用扩展安装  
```
# install后可跟多个扩展名，按需安装
apt-get install php5-mcrypt php5-gd php5-curl
```
## php-fpm安装  
```
apt-get install php5-fpm
```
## mysql5.6安装  
```
apt-get install mysql-server
```
