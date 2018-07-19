---
title: Node开启一个HTTP服务器
description: "最简单的"
date: 2018-07-19 10:16:51
tags:
- http
- Node
categories:
- 技术学习
---
``` js

var http = require ('http');

http.createServer(function(request, response){}).listen(3000);
console.log('server has started...');

```

这样就成功开启了http的服务器，还可以看到命令台输出'server has started'。