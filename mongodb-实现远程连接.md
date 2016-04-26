---
title: mongodb 实现远程连接
date: 2016-04-25 12:32:23
tags:
---
## mongodb 实现远程连接


mongodb远程连接配配置，分以下4步。
### 1、添加管理员账

    > use admin
    switched to db admin
    > db.addUser('tank','test');
### 2、配置mongodb.conf

    #bind_ip = 127.0.0.1   //注释此行
    auth = true       //将此行前的注释去掉

### 3、重启mongodb

     /etc/init.d/mongod
### 4、防火墙开放27017端口
    iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 27017-j ACCEPT
### 5、测试
<img src="/mongodb-实现远程连接/2014722101724369.png">
