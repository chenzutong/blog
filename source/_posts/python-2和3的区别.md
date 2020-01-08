---
title: python 2和3的区别
date: 2020-01-04 10:22:30
categories:
  - Python
  - 其他
tags:
---

## python 2.x和3.x的区别

* 在python 2.x中， print语句被Python 3.x中的print()函数所代替
* 在Python 3.x中，整数之间的相除("/")，结果是浮点数，而在Python 2.x中结果是整数
* Python 3.x源码文件默认使用UTF-8编码，所以支持直接写入的中文，而Python 2.x默认编码是ASCII,直接写入中文会被转换为ANSI编码。
* 在Python 3.x中将range()与xrang()函数整合为一个range()函数，所以Python 3.x中不存在xrange()函数