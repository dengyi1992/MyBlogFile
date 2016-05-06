---
title: android 防手抖
date: 2016-05-06 12:06:06
tags:
---

## android 防手抖

#### 应用场景

* 按钮点击不能太快
* 数据更新不能太过于频繁


前些天用socket.io做推送更新数据库的时候，由于每次点击发送的时候会收到多条更新数据，所以想到了这个补全之策，因为服务器端发送多次的原因暂时找不到，所以先在android端做处理先。代码如下：


      /**
      * Created by deng on 16-4-28.
      * /

      public class ForbinMultiClick {
      private static long lastClickTime;
      public synchronized static boolean isTooQuick() {
      long time = System.currentTimeMillis();
      if ( time - lastClickTime < 500) {
          return true;
      }
      lastClickTime = time;
      return false;
      }
      }
