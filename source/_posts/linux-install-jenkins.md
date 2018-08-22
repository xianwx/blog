---
title: Ubuntu Linux 16.04下如何安装Jenkins
description: "人人都爱Jenkins"
date: 2018-07-23 15:58:35
tags:
- Linux
- Jenkins
categories:
- 技术学习
---
# 安装Java
{% post_link linux-install-java Ubuntu Linux 16.04下如何安装Java %}

# 安装Jenkins
很简单，只需要四个命令。
```sh
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d
sudo apt-get update
sudo apt-get install jenkins
```
（如果出现"subprocess installed post-installation script returned error exit status 1"，看另外一篇。）
等待安装完毕，即可反问Jenkins，默认端口8080，配置文件在`/etc/default/jenkins`里。
此时通过 ip:8080可以访问Jenkins，初次访问界面上会提供一个地址文件让你去看密码，取的密码后，新建一个admin的用户，就可以欢快的使用Jenkins了。