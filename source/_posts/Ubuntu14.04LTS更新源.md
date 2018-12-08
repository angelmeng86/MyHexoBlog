---
title: Ubuntu14.04LTS更新源
categories:
  - 操作系统
date: 2016-06-15 19:39:36
tags:
  - linux
---

_原文_  [http://www.cnblogs.com/stone-dan-dan/p/4246027.html](http://www.cnblogs.com/stone-dan-dan/p/4246027.html?utm_source=tuicool&utm_medium=referral) 第一步：备份源列表 sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup

#### 第二步：

打开源列表并进行更改（我用gedit命令）： 此处附上我的源列表（取您所需）： 官方源：

deb http://archive.ubuntu.com/ubuntu/ trusty main restricted universe multiverse deb http://archive.ubuntu.com/ubuntu/ trusty-security main restricted universe multiverse deb http://archive.ubuntu.com/ubuntu/ trusty-updates main restricted universe multiverse deb http://archive.ubuntu.com/ubuntu/ trusty-proposed main restricted universe multiverse deb http://archive.ubuntu.com/ubuntu/ trusty-backports main restricted universe multiverse deb-src http://archive.ubuntu.com/ubuntu/ trusty main restricted universe multiverse deb-src http://archive.ubuntu.com/ubuntu/ trusty-security main restricted universe multiverse deb-src http://archive.ubuntu.com/ubuntu/ trusty-updates main restricted universe multiverse deb-src http://archive.ubuntu.com/ubuntu/ trusty-proposed main restricted universe multiverse deb-src http://archive.ubuntu.com/ubuntu/ trusty-backports main restricted universe multiverse deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse

163源：

deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse

阿里源：

deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse

centos源（放心使用）：

deb http://centos.bitcomm.cn/ubuntu trusty main restricted universe multiverse deb http://centos.bitcomm.cn/ubuntu trusty-security main restricted universe multiverse deb http://centos.bitcomm.cn/ubuntu trusty-updates main restricted universe multiverse deb http://centos.bitcomm.cn/ubuntu trusty-backports main restricted universe multiverse deb http://centos.bitcomm.cn/ubuntu trusty-proposed main restricted universe multiverse deb-src http://centos.bitcomm.cn/ubuntu trusty main restricted universe multiverse deb-src http://centos.bitcomm.cn/ubuntu trusty-security main restricted universe multiverse deb-src http://centos.bitcomm.cn/ubuntu trusty-updates main restricted universe multiverse deb-src http://centos.bitcomm.cn/ubuntu trusty-backports main restricted universe multiverse deb-src http://centos.bitcomm.cn/ubuntu trusty-proposed main restricted universe multiverse

搜狐源：

deb http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse deb http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse deb http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse deb http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse deb http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse deb-src http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse deb-src http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse deb-src http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse deb-src http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse deb-src http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse

第三步：选择好你要的源并且复制到你的源列表之后，关闭列表，使用sudo apt-get update命令更新 第四步：sudo apt-get upgrade 提示：如果使用虚拟机可以支持进行复制粘帖这些操作，那么可以进行该操作，如果像我一样使用virtualbox的用户或者是使用的是server版Linux的用户怎么办呢？ 建议：你使用putty进行远程操作，因为你习惯了之后你会发现putty可以进行复制黏贴之类的还有就是省去了老是在虚拟机和真实主机之间的频繁切换的操作。更方便。 最后，附上我参考的地址： [http://wiki.ubuntu.org.cn/Qref/Source](http://wiki.ubuntu.org.cn/Qref/Source)