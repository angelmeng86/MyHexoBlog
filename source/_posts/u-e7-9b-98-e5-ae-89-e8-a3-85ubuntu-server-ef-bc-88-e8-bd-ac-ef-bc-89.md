---
title: U盘安装Ubuntu Server（转）
tags:
  - 系统安装
url: 86.html
id: 86
categories:
  - Linux
date: 2016-06-15 19:26:38
---

在网上找了很多教程，都不起效，提示：“从光盘上读取数据出错”。 总结出了几个关键点。 首先，版本，[Ubuntu](http://www.centos.bz/category/other-system/ubuntu/ "Ubuntu") 12.04 Server，一般的U盘安装都会报：“从光盘上读取数据出错”。如果是桌面版(Desktop)，则可以正常安装。 其次，ISO转化成U盘的安装工具，选择win32diskimager，其他工具都会转化的时候可以正常制作成功，但是在安装过程会报：“从光盘上读取数据出错”。在这两个点上面，我折腾了很久，用了很多尝试方法，包括： 无光驱U盘安装 ubuntu server 12.04.1 跳过光驱检测的方法 完成之后打开U盘目录下的isolinuxsyslinux.cfg，将default vesamenu.c32注释为 # default vesamenu.c32 按了F6后面有启动参数添加栏，在后方输入：install cdrom-detect/try-usb=true 并回车，进入安装 以上这些方法都是坑爹的。 只需要选择win32diskimager制作U盘安装程序，就可以正常安装Ubuntu 12.04 Server。 win32diskimager是一款绿色软件，无需安装。解压后运行exe文件即可，界面如下： [![Wdi1](http://www.centos.bz/wp-content/uploads/2013/03/Wdi1.jpg "U盘安装 <wbr>Ubuntu <wbr>从光盘上读取数据出错")](http://www.centos.bz/wp-content/uploads/2013/03/Wdi1.jpg) 文本框用来输入文件完整地址，后面的文件夹图标是浏览窗口，默认只能识别img文件。只需要将iso文件全路径输入在Image File中。 填好镜像的完整地址后右边有个下拉列表用来选择移动设备，千万别选错了！建议只插一个U盘，以免误操作。 之后点击Wirte按钮就开始写入了。写入后重启，从U盘启动就可以进入Ubuntu 12.04 Server安装程序了！