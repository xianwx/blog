---
title: 如何不需要每次输密码访问一台机器
description: "我从山中来，要到你屋去，你屋门紧锁，来去任自如！"
date: 2018-08-15 10:53:15
tags: 
- Linux
- mac
categories:
- 技术学习
---
其实很简单，就是把自己电脑生成的pub公钥传到那台机子上，利用命令`ssh-copy-id`就可以。
如何生存公钥可以查看我的另一篇文章{% post_link manage-your-git-ssh-key 更好的管理你的git SSH-Key %}
但是这个命令在不同的系统，不同的系统版本下参数的要求不一样，具体可以自己看help。
比如我的机子上：
```sh
ssh-copy-id user@remote_ip .ssh/id_rsa.pub
```
然后输入密码，就可以直接用ssh访问那台机子，不要小看这个不需要输密码，它并不只是为了方便，而是可以在脚本里可以用ssh-copy之类的命令了。尤其是向gitlab-runner这种地方，如果不那么弄可能会有问题，之前跟日本方面运维对接，把我们游戏的热更资源先传到中转机再传到最终的cdn上，最开始的做法是传到中转机后然后ssh执行中转机上的一个expect命令，本地写好脚本，测试，OK，没问题。但是在runner里跑就一定失败。。。。最后的做法就是我给中转机和cdn机子加了互信，可以直接ssh连接，然后我脚本改成先ssh到中转机，再ssh-copy到cdn机子。