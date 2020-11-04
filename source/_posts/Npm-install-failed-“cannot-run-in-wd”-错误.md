---
title: Npm install failed “cannot run in wd” 错误
date: 2019-02-01 00:53:46
tags: 
- npm
categories:
- 前端
---

使用 npm install 时，出现了 “cannot run in wd” 等字样时，需要加上 --unsafe-perm 参数，完整如下：

```sh
$ npm install --unsafe-perm
```

官方解释：

> If npm was invoked with root privileges, then it will change the uid to the user account or uid specified by the user config, which defaults to nobody. Set the unsafe-perm flag to run scripts with root privileges.

参考链接：[npm install failed “cannot run in wd”](https://blog.csdn.net/zgljl2012/article/details/52047339)