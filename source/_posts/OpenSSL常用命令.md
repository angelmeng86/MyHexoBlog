---
title: OpenSSL常用命令
tags:
  - openssl
categories:
  - 开发工具
date: 2016-08-31 15:54:02
---

把自己经常使用的openssl命令进行记录，方便查阅

### x509证书格式转换 DER-PEM

```
openssl x509 -inform DER -outform PEM -in cert.der -out cert.pem
openssl x509 -inform PEM -outform DER -in cert.pem -out cert.der
```
命令帮助：
```
openssl x509 -h
```

### Pkcs7 p7b 证书链使用

```
openssl crl2pkcs7 -nocrl -certfile cert1.pem -certfile cert2.pem -certfile cert3.pem -out certchain.p7b
```
命令帮助：
```
openssl crl2pkcs7 -h
```
