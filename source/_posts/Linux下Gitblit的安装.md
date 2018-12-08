---
title: Linux下Gitblit的安装
tags:
  - gitblit
categories:
  - 开发工具
date: 2017-03-05 16:59:02
---

直接进入正题（下列操作都是以root用户执行）：

### 1、去官网下载gitblit-1.8.0.tar.gz，我使用的是Go语言版本；

### 2、解压下载包

tar -xvf gitblit-1.8.0.tar.gz

### 3、把解压的目录移动并改名

mv gitblit-1.8.0 /opt/gitblit

### 4、编辑配置文件

vim /opt/gitblit/data/defaults.properties 我们只需要关注server.httpPort和server.httpsPort，就是访问的Web端口

### 5、配置完成后可以执行脚本，初始化数据

cd /opt/gitblit/ ./gitblit.sh 通过浏览器能够正常访问gitblit，这一步已经完成了基本部署。

### 6、配置为服务，我使用的是Centos系统，执行如下

cd /opt/gitblit ./install-service-centos.sh

### 7、启动服务

service gitblit start 大功告成，谢谢。