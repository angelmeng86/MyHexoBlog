---
title: 在Mac OS上编译 SQLCipher
tags:
  - sqlcipher
url: 268.html
id: 268
categories:
  - C/C++
  - Mac OS
date: 2017-03-31 22:38:49
---

在sqlcipher官网给的方法没法在Mac OS上编译，所以做了以下记录，大家应该明白编译这东西是为了进行加密数据库的脱密或者加密。 ./configure --enable-load-extension --enable-tempstore=yes --with-crypto-lib=commoncrypto CFLAGS="-DSQLITE\_HAS\_CODEC -DSQLITE\_ENABLE\_FTS3" LDFLAGS="/System/Library/Frameworks/Security.framework/Versions/Current/Security" make