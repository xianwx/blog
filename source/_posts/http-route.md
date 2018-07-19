---
title: Node使用路由的方式处理请求
description: "你干你的，我干我的，互不相干。"
date: 2018-07-19 11:03:58
tags:
- http
- Node
categories:
- 技术学习
---

```js
var auth = require('./auth');
http.createServer(function(request, response){
    var pathname = url.parse(request.url).pathname;
    var routeurl = {
        '/auth' : auth.auth
    }

    connection.connect();

    if( typeof routeurl[pathname]=== 'function' ){
        routeurl[pathname](request, response);
    }else{
        console.log('404 not found!');
        response.end();
    }
}).listen(3000);
console.log('server has started...');
```
这样可以单独在auth文件里处理auth的逻辑，不用写一大堆东西在一个文件里了。