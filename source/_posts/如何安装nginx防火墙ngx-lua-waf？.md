---
title: 如何安装nginx防火墙ngx_lua_waf？
date: 2020-11-21 23:31:25
tags: nginx
categories:
- 运维相关
---

ngx_lua_waf是一个基于ngx_lua的web应用防火墙，默认规则有防sql注入、防webshell上传、屏蔽异常网络请求等功能，也可以自定义规则。

<!-- more -->

首先安装Nginx模块ngx_lua，以lnmp一键安装包为例，新增Nginx模块操作如下：  
```
# 1.修改lnmp安装包下 lnmp.conf 文件 Enable_Nginx_Lua 后面 n 改成 y 保存
# 2.升级nginx，输入当前版本号或更新的nginx版本号（注意先备份Nginx网站配置文件）
./upgrade.sh nginx
```

其次安装ngx_lua_waf，下载地址：<https://github.com/loveshell/ngx_lua_waf>，安装步骤如下：
```
# 假设Nginx配置路径如下：/usr/local/nginx/conf/

# 1. 在conf目录下新增waf目录，把ngx_lua_waf解压至waf目录下
# 2. 在nginx.conf的http段添加：
lua_package_path "/usr/local/nginx/conf/waf/?.lua";
lua_shared_dict limit 10m;
init_by_lua_file  /usr/local/nginx/conf/waf/init.lua; 
access_by_lua_file /usr/local/nginx/conf/waf/waf.lua;

# 3. 新增ngx_lua_waf拦截日志（在Nginx安装目录下的logs文件夹下）：
cd /usr/local/nginx/logs
sudo mkdir waf
sudo chown www:www waf # 其中www:www为Nginx运行用户组

# 4. 重启Nginx生效
service nginx restart
```

参考链接：
<https://github.com/loveshell/ngx_lua_waf>
<https://bbs.vpser.net/forum.php?mod=viewthread&tid=18287&highlight=lua>
<https://lnmp.org/faq/lnmp1-2-upgrade.html>