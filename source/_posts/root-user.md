---
title: Linux root用户
description: "我是老大，你们都得听我的。"
date: 2018-07-18 18:22:19
tags: Linux
categories:
- 技术学习
---
有时候在Linux上执行命令会碰到"Permission denied, are you root?"这种报错，这代表我们需要进入root用户执行命令，可以使用以下命令进入root
```sh
sudo -i
```

在root用户状态下，输入命令exit退出root用户状态。

尤其跟别人共用一台云服务器的时候，好刺激。