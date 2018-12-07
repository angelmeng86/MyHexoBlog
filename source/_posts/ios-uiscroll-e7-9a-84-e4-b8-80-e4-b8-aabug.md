---
title: iOS UIScroll的一个BUG
tags:
  - iOS
url: 221.html
id: 221
categories:
  - iOS
date: 2016-12-12 20:18:01
---

之前发现了个UIScroll的BUG，让我郁闷半天，就是当继承该类时，如果使用init方法初始化，其将依次调用initWithFrame和init。真的很让人费解。