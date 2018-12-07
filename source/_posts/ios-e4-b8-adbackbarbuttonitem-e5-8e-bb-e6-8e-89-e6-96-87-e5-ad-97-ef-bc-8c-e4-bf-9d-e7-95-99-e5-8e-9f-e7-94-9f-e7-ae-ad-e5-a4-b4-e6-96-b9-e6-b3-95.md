---
title: iOS中backBarButtonItem去掉文字，保留原生箭头方法
tags:
  - iOS
url: 124.html
id: 124
categories:
  - iOS
  - Object-C
date: 2016-07-31 20:38:40
---

self.navigationItem.backBarButtonItem = \[\[UIBarButtonItem alloc\] initWithTitle:@"" style:UIBarButtonItemStylePlain target:nil action:nil\];