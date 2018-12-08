---
title: Linux常用命令
tags:
  - linux
  - shell
categories:
  - 基础开发知识
date: 2016-08-23 09:52:46
---

0x01 安装包安装卸载
------------

sudo apt-get install google-chrome-unstable
sudo apt-get remove --purge google-chrome-unstable
sudo apt-get autoremove

0x02 监控文件更新信息，用于查看日志文件
----------------------

tail -f /var/log/messages

0x03 开启root用户权限
---------------

sudo passwd root

0x04 ssh开启root远程登录权限
--------------------

sudo vi /etc/ssh/sshd_config
将PermitRootLogin without-password 修改为PermitRootLogin yes
重启ssh服务
service ssh restart