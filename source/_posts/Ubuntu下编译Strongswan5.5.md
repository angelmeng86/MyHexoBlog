---
title: Ubuntu下编译Strongswan5.5
tags:
  - ipsec
  - strongswan
  - ubuntu
categories:
  - 服务端开发
date: 2016-08-22 12:00:09
---

0x00 下载源码
---------

首先去官网Download

wget https://download.strongswan.org/strongswan-5.5.0.tar.bz2

tar -xvf strongswan-5.5.0.tar.bz2

0x01 系统环境
---------

Ubuntu 14.04TLS

0x02 编译依赖
---------

sudo apt-get install libssl-dev libgmp-dev

0x03 编译配置
---------

./configure --prefix=/usr --sysconfdir=/etc --enable-openssl --enable-nat-transport --disable-mysql --disable-ldap --disable-static --enable-shared --enable-md4 --enable-eap-mschapv2 --enable-eap-aka --enable-eap-aka-3gpp2 --enable-eap-gtc --enable-eap-identity --enable-eap-md5 --enable-eap-peap --enable-eap-radius --enable-eap-sim --enable-eap-sim-file --enable-eap-simaka-pseudonym --enable-eap-simaka-reauth --enable-eap-simaka-sql --enable-eap-tls --enable-eap-tnc --enable-eap-ttls

0x04 编译安装
---------

make

sudo make install

0x05 配置使用
---------

请参考[https://zh.opensuse.org/SDB:Setup\_Ipsec\_VPN\_with\_Strongswan](https://zh.opensuse.org/SDB:Setup_Ipsec_VPN_with_Strongswan)