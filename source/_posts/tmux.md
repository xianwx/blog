---
title: tmux的使用
description: "对Linux宝具，tmux咖喱棒！"
date: 2018-07-18 18:16:47
tags: tmux
categories:
- 技术学习
---
最近在给猫与剑做服务器，租了一台腾讯云服务器，要在上边搭建，每次ssh过去发现看不到log，不好创建tab等等问题，于是找了这个神器。

# 安装
在Mac上也可以使用tmux，想尝鲜可以在Mac上先安起来，安装方式：
```sh
brew install tmux
```

# 使用
tmux简单来说就是终端里的『窗口管理器』，如果我使用终端登录到远程主机并运行前台程序，那么这个窗口等于就被占用了，想要看一下 CPU 的使用率，就得再连接一次。但是如果在远程主机上运行 tmux，那么就可以开启多个控制台（类似于窗口），相当高效。如果想查看之前执行命令的log，比如redis的输出等，就很方便了。

Seesion 可以有效地分离工作环境。
* tmux new -s session_name
创建一个叫做 session_name 的 tmux session

* tmux attach -t session_name
重新开启叫做 session_name 的 tmux session

* tmux switch -t session_name
转换到叫做 session_name 的 tmux session

* tmux list-sessions
列出现有的所有 session

* tmux detach (prefix + d)
离开当前开启的 session

我使用tmux new -s work创建一个叫work的session，关闭当前标签页，再使用tmux attach -t work，就可以重新看到之前的tmux session。

Tmux 有一个含有标签的界面，但是它命名这些标签为 “Windows”。为了保持有序，可以重新命名所有我使用的 windows；

* tmux new-window (perfix + c)
创建一个新的 window

* tmux select-window -t :0-9 (perfix + 0-9)
根据索引转到该 window

* tmux rename-window (perfix + ,)
重命名当前 window

perfix为tmux的命令行模式，默认是Ctrl+b，比如new window，可以先按Ctrl+b，然后输入c。

tmux模式下，tmux会监管鼠标的滚动事件。所以没法使用鼠标上下滚动看屏幕输出，这时候可以：
* Ctrl+b，输入:，即可进入命令行模式。
* 输入命令：
```
setw -g mouse on
```

如果以上命令想使用配置文件配置，load配置文件的时候出现unknown option: mode-mouse错误；
可以用

set-option -g mouse on
代替以上代码就OK了！

# 参考
参考资料：
* [Tmux 快速教程](http://blog.jeswang.org/blog/2013/06/24/tmux-kuai-su-jiao-cheng/)
* [tmux指南](https://wdxtub.com/2016/03/30/tmux-guide/)
