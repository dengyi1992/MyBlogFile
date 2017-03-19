---
title: Android 通知栏bug
date: 2017-03-19 11:24:57
tags:
---

前些日对安卓支持的向量图很是感兴趣，所以只要是图标，都去http://iconfont.cn/ 上下载，所有的图标颜色都由自己定,就连lancher也是向量图。
可是直到某一天,用到通知时，只要一弹出通知，整个手机都不能好好工作了，所有的东西都黑屏了，也不能退出，只能关机处理。纠结了很久，以为是自己的手机坏了，换别人的也是如此。

后续发现如此错误

       android.graphics.drawable.VectorDrawable cannot be cast to android.graphics.drawable.BitmapDrawable

说是强转错误，网上也没有搜到这个问题。 算是Android系统的一个bug吧！
这个错误是使用verctor作为icon造成的，使用notification的时候会自动使用lancher的图标，必定有一个转换的过程，算是Google的bug。
