---
title: ionic 安装问题汇总
tags:
  - ionic
categories:
  - 混合开发
date: 2017-02-14 22:33:37
---

一、ionic info出现如下警告 
===
```
****************************************************** 
Dependency warning - for the CLI to run correctly, it is highly recommended to install/upgrade the following: Please install your Cordova CLI to version  >=4.2.0 \`npm install -g cordova\` 
****************************************************** 
```

解决办法： 

```
sudo npm install -g cordova@6.0.0   
```
二、ios-deploy无法安装 解决办法：
===
```
sudo npm install -g ios-deploy --unsafe-perm=true   
```

三、ionic run ios --device真机调试失败 
===
解决办法： 
进入到项目目录platform/ios中，打开xcode项目，然后配置所使用的开发者账号和签名