---
title: github 实现nodejs自动部署
date: 2016-11-01 23:05:46
tags:
---
###  github 实现nodejs自动部署

---

* 编写deploy.sh脚本

      #!/bin/bash

      LOG_FILE="/home/deng_deploy.log"

      date >> "$LOG_FILE"
      echo "Start deployment" >>"$LOG_FILE"
      cd /phpstudy/www/GuideMongo/
      echo "pulling source code..." >> "$LOG_FILE"
      git pull
      npm i
      forever restart /phpstudy/www/GuideMongo/bin/www
      echo "Finished." >>"$LOG_FILE"
      echo >> $LOG_FILE

* 编写deploy.js NodeJs脚本

        var http = require('http')
        var createHandler = require('github-webhook-handler')
        var handler = createHandler({ path: '/incoming', secret: '{密码}' })
        // 上面的 secret 保持和 GitHub 后台设置的一致

        function run_cmd(cmd, args, callback) {
          var spawn = require('child_process').spawn;
          var child = spawn(cmd, args);
          var resp = "";

          child.stdout.on('data', function(buffer) { resp += buffer.toString(); });
          child.stdout.on('end', function() { callback (resp) });
        }

        http.createServer(function (req, res) {
          handler(req, res, function (err) {
            res.statusCode = 404
            res.end('no such location')
          })
        }).listen(7777)

        handler.on('error', function (err) {
          console.error('Error:', err.message)
        })

        handler.on('push', function (event) {
          console.log('Received a push event for %s to %s',
            event.payload.repository.name,
            event.payload.ref);
          run_cmd('sh', ['./deploy.sh'], function(text){ console.log(text) });
        })

        /*
        handler.on('issues', function (event) {
          console.log('Received an issue event for % action=%s: #%d %s',
            event.payload.repository.name,
            event.payload.action,
            event.payload.issue.number,
            event.payload.issue.title)
        })
        * /


* 在服务器上安装coding-webhook-handler
> npm i -S coding-webhook-handler

* 给服务器上deploy.sh 赋予执行权限
> chown 777 deploy.sh

* 用forever 执行deploy.js
> forever start /deploy.js

* 在github setting 设置处做相应设置

在webhooks 添加如下数据

> links : http://ip:7777/incoming

> secret : 密钥

* push代码到github上，看是否有改变
