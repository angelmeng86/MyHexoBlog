---
title: Cordova插件自定义iOS版
tags:
  - Cordova
categories:
  - 混合开发
date: 2017-02-19 16:38:25
---

Cordova版本：6.0.0

一、参考官网搭建基本环境
------------

[http://cordova.apache.org/#getstarted](http://cordova.apache.org/#getstarted) 1、npm install -g cordova 2、cordova create helloPlugin 3、cordova platform add ios 4、cordova run ios 按照上述操作已经搭建好了一个测试环境，我们做好后的插件就是在helloPlugin项目中做测试。

二、继续参考官方文档搭建插件管理环境
------------------

[http://cordova.apache.org/docs/zh-cn/6.x/plugin_ref/plugman.html](http://cordova.apache.org/docs/zh-cn/6.x/plugin_ref/plugman.html) 1、npm install -g plugman 2、 plugman create --name dialog --plugin\_id cordova-plugin-dialog --plugin\_version 0.1 --variable author=Maple 上述命令执行完，我们已经创建了一个插件目录。

三、编写iOS版插件，其它平台类似
-----------------

[http://cordova.apache.org/docs/zh-cn/6.x/guide/platforms/ios/plugin.html](http://cordova.apache.org/docs/zh-cn/6.x/guide/platforms/ios/plugin.html) 1、参考官方文档创建一个iOS插件类： HndevDialog.h:

#import <UIKit/UIKit.h>

#import <Cordova/CDVPlugin.h>

@interface HndevDialog : CDVPlugin

\- (void)showMessageBox:(CDVInvokedUrlCommand*)command;

@end

HndevDialog.m:

#import "HndevDialog.h"

@implementation HndevDialog

\- (void)showMessageBox:(CDVInvokedUrlCommand*)command
{
    CDVPluginResult* pluginResult = nil;
    if (command.arguments.count == 2) {
        NSString* title = \[command.arguments objectAtIndex:0\];
        NSString* message = \[command.arguments objectAtIndex:1\];
        UIAlertView *alertView = \[\[UIAlertView alloc\] initWithTitle:title message:message delegate:nil cancelButtonTitle:@"Cancel" otherButtonTitles:nil, nil\];
        \[alertView show\];
        pluginResult = \[CDVPluginResult resultWithStatus:CDVCommandStatus_OK\];
    }
    else
    {
        pluginResult = \[CDVPluginResult resultWithStatus:CDVCommandStatus_ERROR messageAsString:@"Arg was invalid."\];
    }
    \[self.commandDelegate sendPluginResult:pluginResult callbackId:command.callbackId\];
}

@end

把上述两个文件放到插件工程dialog/src/ios/目录下。编写上述类时可以使用xcode开发工具进行编写，编译没问题再直接拷过来。 2、修改dialog/plugin.xml文件

<?xml version='1.0' encoding='utf-8'?>
<plugin id="cordova-plugin-dialog" version="0.1" xmlns="http://apache.org/cordova/ns/plugins/1.0" xmlns:android="http://schemas.android.com/apk/res/android">
 <name>dialog</name>
 <AUTHOR>Maple</AUTHOR>
 <js-module name="dialog" src="www/dialog.js">
     <clobbers target="cordova.plugins.dialog" />
 </js-module>
 <platform name="ios">
     <config-file target="config.xml" parent="/*">
         <feature name="HndevDialog">
             <param name="ios-package" value="HndevDialog" />
         </feature>
     </config-file>

     <header-file src="src/ios/HndevDialog.h" />
     <source-file src="src/ios/HndevDialog.m" />
 </platform>
</plugin>

3、修改dialog/www/dialog.js文件

var exec = require('cordova/exec');

exports.show = function(title, msg, success, error) {
    exec(success, error, "HndevDialog", "showMessageBox", \[title, msg\]);
};

到此插件的代码基本完成。

四、安装插件并验证可用性
------------

回到第一章我们创建的helloPlugin项目目录 1、 cordova plugin add /Users/maple/Documents/ionic/dialog 上面的路径就是你的插件绝对路径。 2、在helloPlugin/www/js/index.js增加测试代码:

var app = {
 // Application Constructor
 initialize: function() {
 this.bindEvents();
 },
 // Bind Event Listeners
 //
 // Bind any events that are required on startup. Common events are:
 // 'load', 'deviceready', 'offline', and 'online'.
 bindEvents: function() {
 document.addEventListener('deviceready', this.onDeviceReady, false);
 },
 // deviceready Event Handler
 //
 // The scope of 'this' is the event. In order to call the 'receivedEvent'
 // function, we must explicitly call 'app.receivedEvent(...);'
 onDeviceReady: function() {
     app.receivedEvent('deviceready');
 var dialog = cordova.require('cordova-plugin-dialog.dialog');
 dialog.show('Dialog Title', 'Hello Maple!',function() {
 
 }, function(message) {
 alert(message);
 });
 
 },
 // Update DOM on a Received Event
 receivedEvent: function(id) {
 var parentElement = document.getElementById(id);
 var listeningElement = parentElement.querySelector('.listening');
 var receivedElement = parentElement.querySelector('.received');

 listeningElement.setAttribute('style', 'display:none;');
 receivedElement.setAttribute('style', 'display:block;');

 console.log('Received Event: ' + id);
 }
};

app.initialize();

3、cordova run ios 执行效果如图： ![result](http://www.hndev.cn/wordpress/wp-content/uploads/2017/02/result.jpeg) 大功告成，附上插件源码：[https://github.com/angelmeng86/cordova-plugin-dialog](https://github.com/angelmeng86/cordova-plugin-dialog) 谢谢支持！