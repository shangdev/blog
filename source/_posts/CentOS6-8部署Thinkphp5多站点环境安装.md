---
title: CentOS6.8部署多站点环境安装
date: 2017-03-13 22:01:38
tags: "Linux"
categories: "运维相关"
---
# 背景   
网站做多了，难免会接触到一个又一个的服务器，，而且环境又都不一样，每次部署站点到Linux的CentOS服务器上，总是得为装这装那各种环境而谷歌，所以决定把用到的环境安装方案以及常见的坑列出来，以备后查。  
# 部署环境列表    
   -  lnmp一键安装包，方便，快键；
   -  Composer包管理器；
   -  Node.js版本管理NVM安装以及Git密钥设置；
   -  Swoole的php扩展安装；
   -  Git初始化设置；
   -  Redis安装；
   -  ...  

<!-- more -->

# 具体步骤  
## lnmp一键安装包  
这个没啥好说的吧，[lnmp官网地址](http://www.lnmp.org/)，有详细的安装步骤，唯一需要注意的应该是**安装前**需要先安装screen，因为如果是在本地虚拟机里新建的Linux服务器，默认是不安装screen的，方法如下：  
```
yum -y install screen    
#yum是centos安装软件的方法，unbuntu下是apt-get
```
## Composer包管理器安装  
```
#下载安装
curl -sS https://getcomposer.org/installer | php

#移动composer.phar到/usr/local/bin目录下
mv composer.phar /usr/local/bin/composer
```
## NVM安装以及Git密钥设置  
见我的另一篇博客[记录一次Oracle VM VirtualBox 的奇妙之旅...](https://my.oschina.net/ShangDev/blog/833998)，有详细的介绍。
## Swoole扩展安装  
首先下载并解压，这里注意，我是在**本地自建的虚拟机**里安装的，所以wget时会失败，那么就需要你亲自下载然后通过ftp上传到虚拟机了。  
下载swoole最新版并解压，cd到解压目录：  
```
wget -c https://github.com/swoole/swoole-src/archive/v2.0.6.tar.gz
tar -zxvf v2.0.6.tar.gz
cd swoole-src-2.0.6

#使用phpize来生成php编译配置
phpize

#./configure 来做编译配置检测,这里需要注意，如果你是通过lnmp一键安装包安装的lnmp环境，那么你需要在./configure之后再加上 --with-php-config=/usr/local/php/bin/php-config,否则将会报错
./configure

#make进行编译，make install进行安装
make && make install

#配置编辑php.ini配置文件，添加swoole扩展，保存之后重启php以及fpm，或者更方便你可以lnmp restart，然后查看phpinfo()，可以看到swoole已经被支持了。
extension swoole.so;
```
## Redis安装  
> 大体步骤和swoole差不多，如下  

```
# 获取Redis
wget http://pecl.php.net/get/redis-3.1.0.tgz
# 解压文件并移动到 /usr/local/redis目录下
tar zxf redis-3.1.0.tgz 
mv redis-3.1.0 /usr/local/redis
cd /usr/local/redis
# 进入目录生成配置文件
phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install
```
通过以上步骤就已经安装了redis-3.1.0了，接下来配置redis文件并设置开机启动  
```
# 设置配置文件路径(/etc/redis)并复制当前目录下redis.conf到/etc/redis下
mkdir -p /etc/redis
cp redis.conf /etc/redis

# 修改配置文件(把其中的deamonize no修改为deamonize yes)
vi /etc/redis/redis.conf
```
现在我们启动redis服务，并设置开机启动  
```
# 启动redis服务
/usr/local/bin/redis-serve /etc/redis/redis.conf

# 查看启动
ps -ef | grep redis

# 设置开机启动
echo "/usr/local/bin/redis-server /etc/redis/redis.conf &" >> /etc/rc.local
```
最后我们在php配置文件进行扩展redis安装  
```
extension=redis.so
```
重启服务：
```
lnmp restart
```
最后查看phpinfo信息，如果看到有Redis信息，则说明安装成功！  

提示：题主最近也变懒了，没有测试就发了博文，实际上以前发的上述的redis并不能正常安装及使用，在此鞭笞已身，再懒剁屌...
