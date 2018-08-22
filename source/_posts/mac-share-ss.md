---
title: Mac通过Privoxy共享shadowsocks代理给其它设备
description: "信不信我的最初目的其实是为了NS可以发截图到Twitter上？"
date: 2018-07-24 11:09:31
tags: 
- mac
categories:
- 技术学习
---
# WHY
ss通过socks5代理连接，属于局部代理。有些软件不支持sock5代理，只支持HTTP代理，或者你想让你的手机翻墙，就可以把本地的socks5代理共享出去，Mac OS本身没有这个功能，所以我们需要借助Privoxy。

# 前置工作
先确定你用的是不是ss，我用的另一个翻墙软件走的就已经是HTTP和HTTPS代理了。。。。如何确认呢？打开网络设置
![107EA2CBA77698A10C6D445BFC50A2F2](/images/107EA2CBA77698A10C6D445BFC50A2F2.jpg)
如果是自动代理配置也行，先打开全局，还需要看看端口。

# 安装
执行命令安装：
```sh
brew install privoxy
```
如果出现报错：
```
The `brew link` step did not complete successfully.
The formula built, but is not symlinked into /usr/local
Could not symlink sbin/privoxy
/usr/local/sbin is not writable.
```

证明你缺少sbin这个文件夹或者文件夹的读写权限不正确，可以使用以下命令：
```sh
sudo mkdir /usr/local/sbin
#(如果你没有这个文件夹的话，需要执行上面这一条)

sudo chown -R '你的用户名':admin /usr/local/sbin/
```
使用这些命令创建以及修改权限结束，再重新link一下：
```sh
brew link privoxy
```

# 配置
配置文件在`/usr/local/etc/privoxy/config`下，习惯用啥修改就用啥修改。
先找到`forward-socks5t  / `去掉注释，改为`forward-socks5t   /               127.0.0.1:1080  .`（我使用的是`forward-socks5   /               127.0.0.1:1080  .`，注意最后的`.`不要漏掉！）
这其中的`1080`端口是ss默认的端口，如果不是，请自己看自己的端口。

再找到`listen-address 127.0.0.1:8118`，去掉注释，改为`listen-address 0.0.0.0:8118`。
`8118`是要开放给其它设备的端口，如果已经使用了，就换一个没被用的。

# 运行

```sh
cd /usr/local/sbin/  
./privoxy –no-daemon /usr/local/etc/privoxy/config &
# 最后的 `&` 是为了让 privoxy 在后台运行
```

如果没有报错，就可以用了。

# 使用
1. 打开手机，与Mac连上同一个WiFi。
2. 修改WiFi配置，把HTTP代理设置为手动
3. 设置服务器为电脑局域网ip
4. 设置端口为配置开放的端口（如上边的8118）
5. 可以翻墙了

# shutdown
先查看进程id：
```sh
ps aux | grep privoxy
```

kill：
```sh
sudo kill 90986
```

# 参考
[共享 macOS 上的 ShadowSocks 代理给其他设备](https://sebastianblade.com/share-macos-shadowsocks-proxy-to-other-device/)
[Mac上配置Privoxy](https://www.cnblogs.com/DeviLeo/p/6033591.html)
[Mac上Privoxy将shadowsocks的socks5代理转为http代理](https://blog.csdn.net/yanzi1225627/article/details/51064306)