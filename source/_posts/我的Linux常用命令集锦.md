---
title: 我的Linux常用命令集锦
date: 2017-03-12 03:15:04
tags: 
- Linux
categories:
- 运维相关
---
# linux进程命令

## ldd

> list, dynamic, dependencies,列出程序所使用的动态函数库的信息

列出ldd的版本号
```
# ldd -version
```

列出指定文件所有依赖

```
# ldd -v [file]
```

## ps命令

> PS是Linux下最常用的也是非常强大的进程查看命令

以下这条命令是检查php进程是否存在.  
1. -e 显示所有进程;  
2. -f 全格式;  
3. grep(Global Regular Expression Print,全局正则表达式版本)是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。 

```
# ps -ef | grep php
```

<!-- more -->

## nuhup

> 不挂断地运行命令  

以下这条命令时使geddy进程挂起在后台运行：  

```
# nohup geddy &
```

此处需注意，后面必须加&符号，然后回车，则在缺省状态下该作业的所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件：  

```
# nohup command > myout.file 2 > &1 &
```

在上面的例子中，输出被重定向到myout.file文件中。  

如果想查看后台已挂起的任务，我们使用jobs来查看任务：  

```
# jobs
```

使用fg来关闭任务：  

```
# fg
```

此处需注意，使用jobs命令在命令台重启后将失效，解决办法是使用screen命令，以后记录。  

## sereen
>用于命令行终端切换的自由软件, 用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。

同样地我还是以centos6.8来进行操作演示：
安装screen

```
# yum install screen -y
```

创建一个新的窗口, 注意：下列的name是为了标记新建窗口的名称，以便识别。亦可以不加。

```
# screen -S name
```

输入你想挂起的命令, 此时我的命令时 php index.php index/Chat/start

```
# php index.php index/Chat/start
```

暂时中断该会话
直接按 ctrl + a + d,提示如下：

```
[detached]
```

重新连接会话

```
找到该会话(以下命令会输出所有会话窗口的id以及信息)
# screen -ls

进入该会话(-r 后面跟的是以上列出你想进入会话的的id)
# screen -r 24414
```

退出按 ctrl + c，当然了我这只是皮毛而已，Google上有各路大神更详细的关于该命令的教程，可以去参考。
