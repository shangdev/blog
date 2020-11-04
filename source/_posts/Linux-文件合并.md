---
title: Linux 文件合并
date: 2019-01-31 02:02:39
tags:
- linux
categories:
- 运维相关
---

把 source 文件夹下的所有文件合并到 target 文件夹下。命令如下：

```sh
$ cp -r -n source/* target/
```

其中：
-r：递归文件夹；
-f：覆盖已存在的目标文件，不给出提示；
-i：覆盖已存在的目标文件之前，给出提示，输入 y 则确认覆盖；
-n：不覆盖已存在的目标文件，直接跳过（使 -i 失效）；
