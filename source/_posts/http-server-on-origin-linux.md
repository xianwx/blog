---
title: 给云服务器安装HTTP服务器
description: "我们不仅没有返回，而且还没有响应。"
date: 2018-07-18 18:33:08
tags:
- http
- server
- Linux
categories:
- 技术学习
---
1. Node的安装
```sh
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
sudo apt-get install -y nodejs
```
这种安装方法的好处是：直接安装在系统环境/usr/bin目录下，之后使用npm -g安装其他插件也会安装到/usr/lib/node_modules(需要使用sudo权限)
2. 软链接到项目文件夹下边
```sh
ln -s /usr/lib/node_modules /YOUR_PROJ/
```
3. 安装MySQL：使用下面三行命令：
```sh
sudo apt-get install mysql-server sudo apt install mysql-client sudo apt install libmysqlclient-dev
```
3. 使用命令检查MySQL的安装是否成功：
```sh
sudo netstat -tap | grep mysql
```
4. 使用以下命令进入MySQL状态：
```sh
mysql -uroot -p你的密码
```
5. 为了可以远程访问MySQL，修改配置：
```sh
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
注释掉 
```
bind-address = 127.0.0.1
```

wq保存退出

6. 重新进入MySQL命令状态，执行以下两条代码：
```sh
grant all on *.* to root@'%' identified by '你的密码' with grant option;
flush privileges;
```
7. quit MySQL命令行，执行重启命令：
```sh
sudo /etc/init.d/mysql restart
```
命令汇总：
```sh
sudo /etc/init.d/mysql start
sudo /etc/init.d/mysql stop
sudo /etc/init.d/mysql restart
```

8. 使用MySQL workbench连接远程数据库执行创建数据库等操作。
9. 给Node安装MySQL插件
```sh
sudo npm install -g mysql
```
10. 启动http服务器