---
title: Windows中Vagrant的安装
date: 2017-03-28 00:07:04
tags: "Linux"
categories: "运维相关"
---
# 准备
## 下载VirtualBox  
下载地址：http://download.virtualbox.org/virtualbox/5.1.12/VirtualBox-5.1.12-112440-Win.exe

## 下载vagrant  
* 官网下载地址：https://www.vagrantup.com/downloads.html  
* 这里附上我的百度网盘分享地址（版本1.9.3）：http://pan.baidu.com/s/1kVDhWYr 

## 下载vagrant box  
* 下载地址：http://www.vagrantbox.es/ （*注意*：直接在浏览器中下载可能会速度很慢，建议在迅雷极速版中下载，淘宝开个会员，没多少钱的对吧。）  
* 这里同样附上我下载好的Box：http://pan.baidu.com/s/1c1G3LEo  （*注意*：我分享的是centos7.2的box，如果你想下载其他的，就进入上面的地址自己查询下载即可）  

<!-- more -->

# 安装  
## VirtualBox安装  
这里就不在赘述了，我的另一篇博客有详细说明：[Oracle VM ViryualBox虚拟机的奇幻之旅](http://thinkuser.cn/2017/03/13/Oracle-VM-ViryualBox%E8%99%9A%E6%8B%9F%E6%9C%BA%E7%9A%84%E5%A5%87%E5%B9%BB%E4%B9%8B%E6%97%85/)  
## vagrant安装  
这里也不赘述，就像平时安装软件一样，选择好安装目录就ok了，不建议装在系统盘。  
## vagrant box安装  
首先，进入到你想挂载vagrant的目录,右键打开Git Bash命令台（只有你电脑安装了Git才有的，如果没有安装请使用windows自带的cmd窗口），  
```
# 下列centos7.2是你给box起的别名，virtualbox.box是你下载的vagrant box包所在目录
vagrant box add centos7.2 virtualbox.box

vagrant init centos7.2

# 虚拟机启动
vagrant up

# 使用vagrant ssh登录虚拟机(注意：虚拟机用户名密码默认都是vagrant)
vagrant ssh
```  
# 结尾  
ok了，Vagrant至此已安装完成，至于其他的指令，待我以后有时间了细细讲来...