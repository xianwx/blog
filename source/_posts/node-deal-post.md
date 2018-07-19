---
title: Node如何处理POST请求
description: "不给参数就捣蛋！"
date: 2018-07-19 11:16:55
tags:
- http
- Node
categories:
- 技术学习
---
``` js
// auth.js
var url = require('url');
var querystring = require('querystring');

function auth(req, res) {

    //暂存请求体信息
    var body = "";
 
    //请求链接
    console.log(req.url);
 
    //每当接收到请求体数据，累加到post中
    req.on('data', function (chunk) {
        body += chunk;  //一定要使用+=，如果body=chunk，因为请求favicon.ico，body会等于{}
        console.log("chunk:",chunk);
    });
 
    //在end事件触发后，通过querystring.parse将post解析为真正的POST请求格式，然后向客户端返回。
    req.on('end', function () {
        // 解析参数
        body = querystring.parse(body);  //将一个字符串反序列化为一个对象
        console.log("body:",body);

        // 设置响应头部信息及编码
        res.writeHead(200, {'Content-Type': 'text/html; charset=utf8'});
        var data = {"code":200, "msg":"success", "token":"token_" + body.username + "_" + body.password};
        res.end(JSON.stringify(data));
    });
}
exports.auth = auth;

```

![](/images/9F401EAC8A9114C972DCC8812E80871D.png)