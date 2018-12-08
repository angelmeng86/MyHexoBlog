---
title: Gitlab源码管理及持续集成
tags:
  - git
categories:
  - 开发工具
date: 2016-06-16 10:06:17
---

0x00 Gitlab安装
====================

Gitlab到目前为止已经发展得很成熟了，可以登录其官方网站进行对于操作系统的安装包下载，基本安装方式都是一键傻瓜安装。 网址：[https://about.gitlab.com/](https://about.gitlab.com/) 
我使用的是Ubuntu 64位系统，安装包gitlab-ce\_8.7.3-ce.0\_amd64.deb，建议安装8.0以上版本，因为该版本可以很好的支持可持续集成功能。

0x01 Gitlab源码管理
=======================

1、首先，登录Gitlab，添加自己的SSH Keys，如图 
![snapshot1](/upload/old/201606snapshot1.png) 

2、创建一个test仓库 ![snapshot2](/upload/old/201606snapshot2.png)    

3、输入如下命令，提交代码至仓库中。
```
$ mkdir test
$ touch main.c
$ vim main.c
```

```
#include <stdio.h>

int main()
{
    printf("Hello GitLab!\\n");
    return 0;
}
```

```
$ git init
$ git add -A
$ git commit -m "git init"
$ git remote add origin  git@192.168.1.5:mapple/test.git
$ ssh-add SSH Key对应的私钥文件
$ git push -u origin master
```
0x02 Gitlab持续集成
===================

首先，在你进行持续集成的服务器上安装如下工具

```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.deb.sh | sudo bash
sudo apt-get install gitlab-ci-multi-runner
```

然后注册
```
sudo gitlab-ci-multi-runner register
```

当中会让你填写一些信息. 例如你的 gitlab-ci coordinator 的地址和注册这个 runner 的 token，这两个在你 GitLab 中可以找到。关于 executor 的话, 我这里使用的是 shell, 因为我将 runner 直接运行在物理机的系统上。如下图，在项目的Settings里，有个Runner选项。 
![snapshot3](/upload/old/201606snapshot3.png) 

上图里面的地址为http://11.12.110.79/ci
token为UsxTMH-QCwxQxsoPLa9y 

![snapshot4](/upload/old/201606snapshot4.png) 
如上，配置完成后可以在界面中看到配置好的Runner。 
![snapshot5](/upload/old/201606snapshot5.png) 
接下来，参考http://docs.gitlab.com/ce/ci/quick_start/README.html
添加.gitlab-ci.yml文件到test仓库根目录下，我的内容如下：

```
before_script:
- java -version

testbuild:
script:
- gcc main.c
- cp a.out ~/
- ./a.out
```

上述代码中，before_script为脚本执行前的准备工作，每一行就相当于在shell中执行一个命令，上述内容的意思就是打印java的版本信息，然后gcc编译main.c文件，拷贝编译出来的a.out至当前用户的主目录下，执行当前编译出来的可执行程序，该文件执行一些命令很灵活，可以进行相应的自动构建编译，然后自动单元测试，最后自动部署。

```
$ git add .gitlab-ci.yml
$ git commit -m "add.gitlab-ci.yml"
$ git push
```

该文件推送到仓库后会自动触发上述自动脚本的执行。 
![snapshot6](/upload/old/201606snapshot6.png) 
基本的用法就讲到这，给大家抛砖引玉啦。