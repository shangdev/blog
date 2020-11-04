title: wp-calypso安装那些事儿
author: bear.shang
date: 2018-01-27 02:27:00
tags: "wordpress"
categories: "wordpress"
---
什么是wp-calypso呢？看这里：[wpcalypso - 古腾堡](https://github.com/Automattic/wp-calypso)。
简单讲这是一个致力于让wordpress后台变得更轻、快、好的前端应用。  
在本地安装wp-calypso时我遇到了各种各样的坑，特此记录。  
# 服务器环境 
## Node
虽然官方使用的node版本是v8.9.3，但是我们把它更新到最新版，截至此时最新版是v8.9.4。使用nvm管理器来对node进行版本管理：
```
# 切换到安装目录
cd ~
# 下载nvm最新版
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
or
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
# 安装
source ~/.bashrc
# 列出所有可用的node
nvm ls -remote
# 下载指定的node
nvm install v8.9.4
# 设置系统默认的node
nvm alisa default v8.9.4
# 查看当前node
node -v
# 也可以直接下载LTS版node
nvm install lts/carbon
```
## gcc
如果你使用centos6.9及以下版本，那么默认gcc版本在v4.4.7以下，包括v4.4.7，现在升级到最新版，截止此时最新版是v7.3.0.
```
# 切换到安装的目录
cd ~
# 下载最新版gcc（截止目前为v7.3.0）
wget ftp://ftp.gnu.org/gnu/gcc/gcc-7.3.0/gcc-7.3.0.tar.gz
tar zxf gcc-7.3.0.tar.gz
cd gcc-7.3.0
# 下载所需依赖包
./contrib/download_prerequisites
# 创建编译后文件存放目录（gcc-build-7.3.0）
mkdir gcc-build-7.3.0
cd gcc-build-7.3.0
# 编译更新
../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib
make
make install
# 重启虚拟机
reboot
# 查看安装的gcc
gcc- v
```
# npm start曲折
执行npm start后可能会有错误发生，如：  
## ‘GLIBCXX_3.4.21’not found  
这是因为升级gcc时，生成的动态库没有替换老版本gcc的动态库导致的，将gcc最新版本的动态库替换系统中老版本的动态库即可解决，如下操作：
```
# 执行以下命令，查找编译gcc时生成的最新动态库：
find / -name "libstdc++.so*"
# 将输出的最新动态库libstdc++.so.6.0.24复制到/usr/lib64目录下：
cp /home/gcc-7.3.0/gcc-temp/stage1-x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.24 /usr/lib64
# 重建默认库的软连接
cd /usr/lib64
rm -rf libstdc++.so.6
ln -s libstdc++.so.6.0.24 libstdc++.so.6
# 重新运行以下命令检查动态库，出现（GLIBCXX_3.4.24）即替换成功
strings /usr/lib64/libstdc++.so.6 | grep GLIBC

```
# Hosts
这里是由于个人原因导致的，因为我是在公司的虚拟机上运行的wp-calypso项目，所以需要对hosts文件做一些手脚：
```
# 修改本地hosts文件，新增行（172.22.11.123为目标虚拟机IP）：
172.22.11.123 dev.calypso.com
# 修改虚拟机hosts文件（vi /etc/hosts）
127.0.0.1 calypso.localhost
# 修改nginx配置文件
# the IP(s) on which your node server is running. I chose port 4000.
upstream dev.calypso.com {
    server calypso.localhost:3000;
    keepalive 1;
}
server
    {
        listen 80;
        #listen [::]:80;
        server_name dev.calypso.com;

        location / {
            proxy_pass http://dev.calypso.com;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        access_log  /home/wwwlogs/dev.calypso.com.log;
    }
# 本地浏览器打开dev.calypso.com即可
```
至此，安装完成了。