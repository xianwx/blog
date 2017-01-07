---
title: 更好的管理你的git SSH-Key
date: 2017-01-07 14:59:38
description: 分别管理公司以及个人的ssh-key
categories: 技术学习
tags: git
---

   日常工作中，我们经常碰到又想写公司的项目，又想自己维护自己的项目，公司的项目基本都在gitlab上，自己的项目在github上，所以我们需要配置不同的SSH-Key来对应不同的环境。

## 1. 设置一个默认的全局name和email
``` bash
git config --global user.name "your name"
git config --global user.email "your email addr"
```
如果不设置这个，已经clone过的每个项目都需要去手动设置一下归属到哪个账户（当然，如果你是用那个clone下来的倒没事，像我是已经clone过了，不得不设置啊）

## 2. 生成SSH-Key
``` bash
$ ssh-keygen -t rsa -C "your company email" -f ~/.ssh/id_rsa
```
一般都叫id_rsa，第一个先弄公司的好了，然后再对应的公司的账户下边加上id_rsa.pub里的内容
再生成github的SSH-Key：

``` bash
$ ssh-keygen -t rsa -C "your github email" -f ~/.ssh/my_rsa
```
同上，把my_rsa.pub里的内容加到github服务器的配置中。

## 3. 添加私钥
``` bash
$ ssh-add ~/.ssh/id_rsa 
$ ssh-add ~/.ssh/my_rsa
```
上面一步执行如果失败，并且提示的是"Could not open a connection to your authentication agent",执行：
``` bash
$ ssh-agent bash
```
添加成功过后，可以执行下面这条查看结果
``` bash
$ ssh-add -l
```
## 4. 修改host配置
打开~/.ssh里的config文件（如果没有就新建一个，没有后缀名）
加上以下内容：
```
# gitlab
Host gitlab.com
    HostName 公司gitlab地址
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
    User 你想要的名字
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/github_rsa
    User 你github要显示的名字
```
测试一下：
``` bash
$ ssh -T git@github.com
```
也可以试试公司的网址，如果出现Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.就可以了。
## 5. 对应账号
如果已经设置了global的用户名和email，就得到不想用默认账户对应的项目文件夹下执行
``` bash
$ git config user.name "your github name"
$ git config user.email "your github email"
```
这样就可以使用github的账号了。如果不想有默认的而且设置过，可以执行
``` bash
$ git config --global --unset user.name
$ git config --global --unset user.email
```