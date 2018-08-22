---
title: 处理"subprocess installed post-installation script returned error exit status 1"报错
description: "挂了挂了，快清理缓存。"
date: 2018-07-24 11:50:58
tags:
- Linux
categories:
- 技术学习
---

# 正文
如果某次使用apt-get出现报错，可能会对之后使用apt-get出现影响，会报"subprocess installed post-installation script returned error exit status 1"，可以用以下方法处理：
```sh
apt-get autoclean
apt-get autoremove
```
就可以卡死的阻断删掉了。

# 参考
[apt-get 出現 “subprocess installed post-installation script returned error exit status 1″](https://actychen.wordpress.com/2010/04/14/apt-get-erro/)

(参考文档里的最后两条命令慎用。。。。我执行了一下，更新了一堆东西。。。。