---
title: Ubuntu Linux 16.04下如何安装Java
description: "不装Java是不可能的，这辈子都不可能的。"
date: 2018-07-23 15:34:57
tags:
- Linux
- Java
categories:
- 技术学习
---
# 搜索可以安装的JDK包
使用命令：
```sh
apt-cache search openjdk
```
![BBDB7F41B79194165AD98AA280D9CC82](/images/BBDB7F41B79194165AD98AA280D9CC82.jpg)

# 安装JDK
sudo apt-get install openjdk-9-jre openjdk-9-jdk

# 配置JAVA_HOME
安装完成可以执行命令来查看安装情况。
```sh
java -version
```
![163CF94DE061CDF1CA25196DF29AE8AF](/images/163CF94DE061CDF1CA25196DF29AE8AF.jpg)

然后在环境变量里设置JAVA_HOME的值，用命令：
```sh
echo "JAVA_HOME=YOUR_JDK_PATH" >> /etc/environment
source /etc/environment
```
最后使用：
```sh
echo $JAVA_HOME
```
来查看环境值设置的对不对。

# 多版本JDK切换
{% post_link alternatives-manage-jdk Linux JDK多版本管理 %}

# 参考
[如何在Ubuntu和Debian中安装OpenJDK Java 9/8](https://www.linuxidc.com/Linux/2017-11/148941.htm)