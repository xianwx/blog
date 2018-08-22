---
title: Linux JDK多版本管理
description: "你其实并不缺管理者"
date: 2018-07-23 15:46:36
tags:
- Linux
- alternatives
- Java
categories:
- 技术学习
---
最近给远程Linux装Jenkins，已经预装好JDK 9，但是在安装Jenkins的时候一直出以下错误：

![3DF39BE8305CF58C4BD4F2A8E64249D4](/images/3DF39BE8305CF58C4BD4F2A8E64249D4.jpg)

重点是：Found an incorrect Java version.原来是我的Java版本跟它要求的不符（妈蛋，我装的最新的啊！

我只好查了一下我装的版本需求的JDK是什么版本，哦，是JDK8.

8 我也有装啊兄弟！我们是自己人啊！干嘛报错啊！
于是运行：
```sh
java -version
```
一检查，openjdk的版本是9，好的吧，我试试直接改环境变量
```sh
echo "JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/" >> /etc/environment
source /etc/environment
```

然而并没有什么卵用。。。。敌人很火力很强，我们快支持不住了！

但系，有个软件可以很方便的管理多版本，那就是——alternatives。

执行命令
```sh
sudo update-alternatives --config java
sudo update-alternatives --config javac
```
执行了用数字序号来选择版本，再次执行
```sh
java -version
```
来查看版本，发现已经被改了，nice！

对alternatives的更多了解，请查看我参考的文章：[linux jdk版本随时切换](http://www.cnblogs.com/fordreamxin/p/4287706.html)