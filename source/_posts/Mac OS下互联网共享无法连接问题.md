---
title: Mac OS下互联网共享无法连接问题
categories:
  - 操作系统
date: 2017-02-20 15:57:18
tags:
  - mac os
---

这个问题折腾我好久了，今天终于解决了，如果你也遇到请试试下列方法： 
### 1、首先进入“系统偏好设置”-》“共享”，修改你的电脑名称，要求修改很简单的字母，如Maple； 
### 2、如果你已经开启共享网络，请先关闭； 
### 3、进入“系统偏好设置”-》“安全性与隐私”，选择“防火墙”，关闭防火墙。 
### 4、删除以下文件：

```
/Library/Preferences/SystemConfiguration/com.apple.nat.plist
/Library/Preferences/SystemConfiguration/NetworkInterfaces.plist
/Library/Preferences/SystemConfiguration/com.apple.airport.preferences.plist
```