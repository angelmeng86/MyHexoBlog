---
title: Centos Linux下部署Parse服务器
tags:
  - parse
  - saas
categories:
  - 服务端开发
date: 2017-03-05 17:42:45
---

0x00 前言
-------

Parse是Facebook很有名的一个项目，能够帮助目前很多应用快速搭建平台快速开发，官网目前已经关闭，并在github提供开源，很好的一个东西。

0x01 环境要求
---------

### 1、MongoDB数据库

1）下载命令如下：
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.4.2.tgz 

tar -xvf mongodb-linux-x86\_64-3.4.2.tgz 2）移动目录，配置环境变量： mv mongodb-linux-x86\_64-3.4.2 /opt/mongodb

export PATH=<mongodb-install-directory>/bin:$PATH 

我的<mongodb-install-directory>就是/opt/mongodb

3）创建数据库目录：
mkdir -p /data/db

/data/db 是 MongoDB 默认的启动的数据库路径(--dbpath)

4）执行mongod命令启动数据库

### 2、Nodejs

    curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
    

    yum -y install nodejs
    

    yum install gcc-c++ make
    
    

[ https://github.com/ParsePlatform/docs](https://github.com/ParsePlatform/docs)