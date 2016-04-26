---
title: Monkey测试简介
date: 2016-04-25 16:21:29
tags:
---
# Monkey测试简介

在android手机上做自动化测试，monkey比cts，Android UnitTest 好用多了，他其实是继承与adb shell中的一段的shell指令。

## 一、monkey测试的相关的原理

monkey测试的原理就是利用socket通讯的方式来模拟用户的按键输入，触摸屏输入，手势输入等，看设备多长时间会出异常。当Monkey程序在模拟器或设备运行的时候，如果用户出发了比如点击，触摸，手势或一些系统级别的事件的时候，它就会产生随机脉冲，所以可以用Monkey用随机重复的方法去负荷测试你开发的软件。

## 二、Monkey程序介绍

*1) Monkey程序由Android系统自带，使用Java语言写成，在Android文件系统中的存放路径是：/sdk/sdk/tools/lib/monkey.jar；相应的途径如图所示：
<img src="/Monkey测试简介/2016-04-25 16:24:44 的屏幕截图.png">


*2) Monkey.jar程序是由一个名为“monkey”的Shell脚本来启动执行，shell脚本在Android文件系统中的存放路径是：/sdk/sdk/tools/bin/monkey；

这样就可以通过在CMD窗口中执行: adb shell monkey ｛+命令参数｝来进行Monkey测试了。

如果我不会用monkey怎么办了？？？


## 三、Monkey命令的简单帮助

要获取Monkey命令自带的简单帮助，在CMD中执行命令：

adb shell monkey –help

这样子，就有他的各种各样的提示命令的参数。

<img src="/Monkey测试简介/2016-04-25 16:30:22 的屏幕截图.png">

我这里对其各种参数进行了简介了。

1) 参数：  -p

参数-p用于约束限制，用此参数指定一个或多个包（Package，即App）。指定

包之后，Monkey将只允许系统启动指定的APP。如果不指定包，Monkey将允许系统启动设备中的所有APP。

* 指定一个包： adb shell monkey -p com.example.sellclientapp  100

说明：com.example.sellClientAPP 为包名，100是事件计数（即让Monkey程序模拟100次随机用户事件）。

* 指定多个包：adb shell monkey -p com.htc.Weather –p com.htc.pdfreader  -p com.htc.photo.widgets 100  如图所示：

<img src="/Monkey测试简介/2016-04-25 16:33:14 的屏幕截图.png">

* 不指定包：adb shell monkey 100

　说明：Monkey随机启动APP并发送100个随机事件
* 要查看设备中所有的包，在CMD窗口中执行以下命令：

        >adb shell
        #cd data/data
        #ls

我的手机没有root所以不能用这个属性了。

2) 参数:  -v

用于指定反馈信息级别（信息级别就是日志的详细程度），总共分3个级别，分别对应的参数如下表所示:

日志级别 Level 0

示例 adb shell monkey -p com.project.zhinan –v 100

说明 缺省值，仅提供启动提示、测试完成和最终结果等少量信息 相应源代码如图所示了，这十分有利于调试了。

日志级别 Level 1

示例 adb shell monkey -p com.htc.Weather –v -v 100

说明  提供较为详细的日志，包括每个发送到Activity的事件信息



日志级别 Level 2

示例 adb shell monkey -p com.htc.Weather –v -v –v 100

说明  最详细的日志，包括了测试中选中/未选中的Activity信息

-s

用于指定伪随机数生成器的seed值，如果seed相同，则两次Monkey测试所产生的事件序列也相同的。

* 示例：

　Monkey测试1：adb shell monkey -p com.htc.Weather –s 10 100

   Monkey 测试2：adb shell monkey -p com.htc.Weather –s 10 100

   两次测试的效果是相同的，因为模拟的用户操作序列（每次操作按照一定的先后顺序所组成的一系列操作，即一个序列）是一样的。操作序



列虽   然是随机生成的，但是只要我们指定了相同的Seed值，就可以保证两次测试产生的随机操作序列是完全相同的，所以这个操作序列伪随



机的；
