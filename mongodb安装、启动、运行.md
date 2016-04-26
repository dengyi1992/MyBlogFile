---
title: mongodb安装、启动、运行
date: 2016-04-25 12:27:23
tags:
---
## mongodb安装、启动、运行

### 1.下载

        https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.2.5.tgz

### 2.安装

tar -zxvf mongodb-linux-x86_64-3.2.5.tgz
   解压完成就OK了。
   cd mongodb-linux-x86_64-3.2.5.tgz进入解压好的目录。把bin目录放到自己想放的位置。
   我是放到/usr/local/mongodb下的。

### 3.启动

      /usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data/ --logpath=/usr/local/mongodb/dblogs --fork

    启动成功：
    about to fork child process, waiting until server is ready for connections.
    forked process: 9150
    all output going to: /usr/local/mongodb/dblogs
    log file [/usr/local/mongodb/dblogs] exists; copied to temporary file [/usr/local/mongodb/dblogs.2014-03-02T21-49-12]

    child process started successfully, parent exiting

    检查是否启动了进程：
    ps aux | grep mongod

    启动命令常用选项说明：
    --dbpath 指定数据库的目录。
    --port 指定数据库端口，模式是27017。
    --bind_ip 绑定IP。
    --derectoryperdb为每个db创建一个独立子目录。
    --logpath 指定日志存放目录。
    --logappend 指定日志生成方式（追加/覆盖）。
    --pidfilepath 指定进程文件路径，如果不指定，将不产生进程文件。
    --keyFile 集群模式的关键标识
    --journal 启用日志
    --nssize 指定.ns文件的大小，单位MB，默认是16M，最大2GB。
    --maxConns 最大的并发连接数。
    --notablescan 不允许进行表扫描
    --noprealloc 关闭数据文件的预分配功能
    --fork 以后台Daemon形式运行服务
    更多的选项利用 mongod --help 进行查看
