---
title: Ubuntu下Android 5.0源码环境搭建
tags:
  - android
categories:
  - 移动端开发
date: 2016-06-15 20:24:40
---

**1、安装ubuntu**

编译Android 5.1需要ubuntu 14.04 TLS 64位的操作系统，在百度上搜索ubuntu，到ubuntu官网下载Ubuntu 64位桌面（desktop）版本，进行安装。安装完成后，需要更新一下ubuntu源。

 Android5.1系统源码编译的磁盘空间要求较高，ubuntu 的磁盘空间需要分配60G以上，内存需要4G以上，否则容易出现编译错误等问题。

**2、安装openjdk-7（编译Android 5.0以上）**

sudo apt-get install openjdk-7-jdk **3、安装编译依赖的软件**

使用如下命令安装依赖软件：

`sudo apt-get install bison build-essential curl flex git gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk2.8-dev libxml2 libxml2-utils lzop maven openjdk-7-jdk pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev mkisofs`

    sudo apt-get install g++-multilib gcc-multilib lib32ncurses5-dev lib32readline-gplv2-dev lib32z1-dev

**4、配置Cache**

sudo apt-get install ccache

source ~/.bashrc