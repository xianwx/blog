---
title: 给远程云服务器部署skynet服务器
description: "不是私服，不是私服，不是私服。"
date: 2018-07-18 18:25:42
tags: 
- Linux
- skynet
- server
categories:
- 技术学习
---
为了给策划能够外网调试，租了一台云服务器用来部署猫与剑的服务器，算是个备忘录吧，以及介绍如何在远程服务器上部署跟skynet有关的服务器。
1. clone 代码
2. 给Ubuntu安装brew，命令：
```sh
sudo apt install linuxbrew-wrapper
```
3. 执行一下brew，等待成功。(不成功多试几次)
4. 执行命令，修改环境变量：
```sh
export PATH="/home/ubuntu/.linuxbrew/bin:$PATH"
```
5. brew前置命令：
```sh
brew doctor
```
6. 安装luarocks，执行命令：
```sh
sudo apt install luarocks
```
7. 安装luarocks，执行命令 ： 
```sh
luarocks install busted
```
8. 安装luacheck，执行命令：
```sh
luarocks install luacheck
```
以上2条命令不行就加上sudo）
9. 下载sx.tar.gz。(在我coding里有存着)
10. 安装skynetx，执行命令：
```sh
sudo tar -ovxzf code/skynetx-ubuntu-v2.0.29.tar.gz -C /usr --strip-components 1
```
11. 我们游戏还需要个HTTP服务器，需要安装Node
[更新]Nodejs的安装使用其它方式，不采用下面做法了。
~~12. 安装Node，命令：sudo apt-get install nodejs~~
~~13. 安装npm，命令：sudo apt-get install npm~~
~~14. 安装完毕，可以执行nodejs —version查看安装结果~~
~~15. 这里使用的nodejs命令比较麻烦，想使用平常用的node命令，可以使用sudo ln -s /usr/bin/nodejs /usr/bin/node来软连接nodejs 到node~~