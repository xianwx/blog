---
title: git常用命令记录
description: "除了使用客户端操作，有时候还需要命令。"
date: 2018-05-19 10:53:12
tags: git
categories:
- 技术学习
---
可能文章的描述会让大家觉得奇怪，程序猿嘛，应该是“除了使用命令操作，有时候还需要客户端。”才对嘛。
但是日常工作中其实，客户端用的居多，毕竟比对啊提交啊什么的，用起来还是很方便很直观的。
此处推荐常用软件：[fork](https://git-fork.com/)
优点是：
* 免费
* 有夜晚主题，不用被骚死tree亮瞎眼
* 速度快
* 一直在维护中，给官方提bug也很乐于解决。
* 从它很不好用一直用到现在，感受到它愈发趋于完善，真的很欣慰。

一般客户端用于：
* commit
* push/pull
* new branch
* fetch
* merge
* stash/use stash
* cherry-pick

以下是命令介绍:
```
git checkout [branch] -- [file name] 重指定分支copy指定文件到当前branch
```
```
git reset --hard [branch]            重置当前到指定分支
```
```
git reset [branch]~[commit times]    丢弃指定分支指定次数的提交
```