---
title: Node连接MySQL
description: "出来混的，总是要存的。"
date: 2018-07-19 11:01:58
tags:
- http
- Node
categories:
- 技术学习
---
# 安装MySQL
## 参考[mysql安装](https://blog.csdn.net/RunIntoLove/article/details/51422787)，装5.7.22版本，注意它提示的密码，保存下来。
## gui的使用参考[MySQL Workbench使用](https://www.jianshu.com/p/dc58a4efdd84)

# Node.js里连接MySQL
```js
var mysql = require('mysql');
var connection = mysql.createConnection({
    host        : '127.0.0.1',
    port        : 3306,
    user        : 'root',
    password    : '5429943',
    database    : 'center'
});

connection.connect();
```

# 相关报错处理
对于报错：`Cannot find module 'mysql' node.js`
只需要执行：
```sh
ln -s /usr/local/lib/node_modules /YOURPROJECTFOLDER/
```

在安装完成MySQL后，先去使用`MySQL Workbench`修改密码。