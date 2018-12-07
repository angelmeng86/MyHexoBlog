---
title: Android应用强制停止工具
tags:
  - Android
url: 180.html
id: 180
categories:
  - Android
  - Java
date: 2016-09-07 12:29:13
---

0x00 背景需求
---------

自己使用的Android定制手机，用了一段时间后总是发现系统响应慢或者死机，所以根据Android系统设置中强制停止应用的原理，写了一个简单小工具，给自己用，也和大家分享一下。

0x01 技术实现
---------

Android系统中Setting设置有个功能，应用程序强制停止，该功能只能执行单个应用的停止，而且由于界面跳转和操作繁琐，对如今我们所使用的手机存在几十个应用的情况，更是像一个鸡肋； 借着这个思路我写一个小程序实现批量的应用程序停止操作，借鉴framework源码，查到该功能使用了ActivityManager.forceStopPackage方法；

0x02 关键代码
---------

//通过传入应用程序包名强制停止该应用，并且不允许该应用自动启动，直到用户手动启动它
private void killProcesses(String packname) {
ActivityManager am = (ActivityManager)this.getSystemService(
Context.ACTIVITY_SERVICE);
am.forceStopPackage(packname);
}

上述方法需要的包名我们可以通过两种方式获取，一种是通过Android的服务接口获取当前所安装的应用，另一种是通过shell命令获取； 下面讲的就是通过shell命令获取的方法：

//获取第三方安装的应用包名列表，可以使用adb shell终端输入看看结果
pm list packages -3

得到包名后就可以结合上述killProcesses方法进行批量应用的停止清理操作了； 具体工程参考：[https://github.com/angelmeng86/AppForceStop](https://github.com/angelmeng86/AppForceStop)