---
title: mongodb 新增字段
date: 2017-03-26 14:05:36
tags:
---

      db.getCollection('businesses').update({}, {$set: {zzcode:"",orgcode:"",type:0}}, {multi: 1})
