---
title: Mac使用记录
description: "使用Mac的时候碰到的一些事情的记录。"
date: 2018-07-18 17:48:48
tags: 
- mac
categories:
- 技术学习
---
为了拥有友好的开发环境，毅然选择了Mac，现在60键的键盘爽到飞起。
扯远了扯远了，还是正经记录吧。

# 命令
如果不管命令执行是否成功都往下执行，可以用;分割多个语句放在一行执行，如果希望前面的执行成功才执行后面的语句，用&&分割语句。

在 fish里 && 用 and代替

在macOS High Sierra里，finder可以用快捷键来显示和隐藏文件夹了：
```sh
Command + Shift + .
```
或者使用命令：
```sh
defaults write com.apple.finder AppleShowAllFiles -bool true
```

# ftp命令
Mac系统10.x过后没有ftp命令，用以下3个命令开起来：

```sh
brew install telnet
brew install inetutils
brew link --overwrite inetutils
```

# MySQL workbench的使用
1\. 创建连接

![](/images/35B48DAD4E40D85B51F961F42CFDDD2A.jpg)

2\. 创建默认的schema
![](/images/14D6CC4556F6F4E73E2FB578D0D2F26E.jpg)

3\. 右键新创建的schema，set as default schema

4\. 可以开始创建table了。

# 将Mac的ss代理共享给其它设备
{% post_link mac-share-ss Mac通过Privoxy共享shadowsocks代理给其它设备 %}