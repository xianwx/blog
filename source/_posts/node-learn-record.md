---
title: Node学习记录
description: "记录自己的无知、愚蠢以及碎碎念。"
date: 2018-07-19 10:22:25
tags:
- http
- Node
categories:
- 技术学习
top: 99
---

# 开启HTTP服务器
{% post_link node-start-server Node开启一个HTTP服务器 %}

# 如何使用curl
{% post_link how-to-use-curl 怎么使用curl模拟http请求 %}

# Node如何使用路由形式
{% post_link http-route Node使用路由的方式处理请求 %}

# Node如何处理POST请求
{% post_link node-deal-post Node如何处理POST请求 %}

# Node如何连接MySQL
{% post_link node-connect-mysql Node连接MySQL %}

# 看书心得
* Node有很多别称，Nodejs，Node.js，NodeJS等，都可以表明它是Node
* 设计高性能Web服务器的几个要点：事件驱动，非阻塞I/O
* ajax的全称是AsynchronousJavascript+XML。即：异步传输+js+XML。是一种技术，只要是这样做的都可以称为ajax请求。
* CommonJS规范的提出。

—————————我是萌萌哒的分割线—————————

* 上下文提供了exports对象用于导出当前模块的方法或者变量，并且是它唯一的导出的出口。
* 在Node中一个文件就是一个模块，在模块中存在一个module对象，它代表模块自身，exports也是module的属性。将方法挂载在exports对象上作为属性即可定义导出的方式。