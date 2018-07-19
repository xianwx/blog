---
title: 怎么使用curl模拟http请求
description: "我不仅仅是个伪装者，我是D员！（嘘）"
date: 2018-07-19 10:55:44
tags:
- http
- Node
categories:
- 技术学习
---
get方式请求：
```sh
curl 地址:端口/路由
```

e.g:
```sh
curl 127.0.0.1:3000/test_sql
```
post方式请求：
```sh
curl -X POST -d "参数名=实参&参数名=实参" 地址:端口/路由
```

e.g:
```sh
curl -X POST -d "username=xianwx&password=test" 127.0.0.1:3000/auth
```