---
title: 学习笔记：PHP7内核剖析（秦朋/著）
date: 2018-07-06 22:55:36
tags: "笔记"
categories: "学习笔记"
---
本文记录在阅读学习**&lt;&lt;PHP7内核剖析&gt;&gt;**过程中的所思所想所感，以备后用。
# 名词解释
## PDO
英文全称：PHP Data Objects，一种在PHP里连接**数据库**的使用接口，她提供了一个数据访问抽象层，这意味着，不管使用哪种数据库，都可以用相同的函数（方法）来查询和获取数据。
<!-- more -->
## PEAR
英文全称：PHP Extension and Application Repository，PHP扩展与应用库。它是一个PHP扩展及应用的一个代码仓库，简单地说，PEAR之于PHP就像是CPAN(Comprehensive Perl Archive Network)之于Perl。
## PECL
英文全称：The PHP Extension Community Library，是一个开放的并通过PEAR打包格式来打包安装的PHP扩展库仓库。通过 PEAR 的 Package Manager 的安装管理方式，可以对 PECL 模块进行下载和安装。
## Native TLS
线程局部存储。
## Cli
英文全称：Command Line Interface，即命令行接口。用于在命令行下执行PHP脚本，就像Shell那样，它是执行PHP脚本最简单的一种方式。
## FastCGI
它是Web服务器（如Nginx、Apache）和处理程序之间的一种通信协议，它是与HTTP类似的一种应用层通信协议。注意：它只是一种协议！
## Fpm
英文全称：FastCGI Process Manager，它是PHP FastCGI运行模式的一个进程管理器。
## ZTS
英文全称：Zend Thread Safety，即Zend线程安全。它是Zend为线程安全机制所提供的用于保证线程的安全，是防止多线程环境下以模块的形式加载并执行PHP解释器，导致内部一些公共资源读取错误，而提供的一种解决方法。
## TSRM
英文全称：Thread Safe Resource Manager，即线程安全资源管理器。
## tsrm_ls
英文全称：TSRM Local Storage，即TSRM存储器。这个是在扩展和Zend中真正被实际使用的指代TSRM存储的变量名。