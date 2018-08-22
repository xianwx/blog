---
title: 对远程服务器利用ssh传输文件
description: "ssh这么好用的吗！"
date: 2018-07-18 18:37:29
tags:
- Linux
- mac
- ssh
categories:
- 技术学习
---
# scp
利用scp传输文件：

传输本地文件到远端：
```sh
scp /path/filename username@servername:/path
```

e.g：
```sh
scp ~/Downloads/skynetx-ubuntu-v2.0.29.tar.gz ubuntu@你猜ip是啥:~/code/
```
然后输个密码就好了。

传输远端文件到本地：
```sh
cp username@servername:/path/filename /var/www/local_dir（本地目录）
```