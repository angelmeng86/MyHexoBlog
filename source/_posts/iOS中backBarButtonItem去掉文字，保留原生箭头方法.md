---
title: iOS中backBarButtonItem去掉文字，保留原生箭头方法
tags:
  - ios
categories:
  - 移动端开发
date: 2016-07-31 20:38:40
---
```
[self.navigationItem.backBarButtonItem = 
    [[UIBarButtonItem alloc] initWithTitle:@"" 
                             style:UIBarButtonItemStylePlain 
                             target:nil action:nil];
```