---
title: “Android设置透明效果”
date: 2016-04-25 17:02:23
tags:
---
# Android设置透明效果

大致分为三种

## 1.用android系统的透明效果

    android:background="@android:color/transparent"

## 2.用ARGB来控制

    Java代码
    半透明<Button android:background="#e0000000" />
    透明<Button android:background="#00000000" />
<img src="/“Android设置透明效果”/layout-2016-04-25-170603.png">
如图文本输入框的值为    
            
      android:background="#22ffffff"


## 3.设置Alpha

Java代码
View v = findViewById(R.id.content);//找到你要设透明背景的layout 的id
v.getBackground().setAlpha(100);//0~255透明度值
