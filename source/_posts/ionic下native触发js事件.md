---
title: ionic下native触发js事件
tags:
  - cordova
  - ionic
categories:
  - 混合开发
date: 2017-02-21 23:09:35
---

说明一下本地层回调js事件的应用场景，推送、后台任务提醒、即时通讯等等；

一、在www/js/app.js中增加如下回调函数：
--------------------------

 

$ionicPlatform.ready(function() {
     if (window.cordova && window.cordova.plugins && window.cordova.plugins.Keyboard) {
         cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
         cordova.plugins.Keyboard.disableScroll(true);

     }
     if (window.StatusBar) {
         // org.apache.cordova.statusbar required
         StatusBar.styleDefault();
     }

     //注册事件函数
     var onReceiveMaple = function(event) {
 alert(event.msg);
 };
 document.addEventListener("hndev.receiveMessage", onReceiveMaple, false);

 });

二、在iOS插件中执行js命令，其它平台类似
----------------------

CDVPlugin.commandDelegate evalJs:\[NSString stringWithFormat:@"cordova.fireDocumentEvent('hndev.%@',%@)", eventName, jsString\]\];

参数eventName为receiveMessage 参数jsString为传入回调函数的对象，必须为json。如 {"msg":"Hello Maple!"}

三、详细可以参考极光推送的实现
---------------

[https://github.com/jpush/jpush-phonegap-plugin](https://github.com/jpush/jpush-phonegap-plugin)