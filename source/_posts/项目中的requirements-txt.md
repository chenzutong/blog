---
title: 项目中的requirements.txt
date: 2019-12-09 20:00:23
categories:
	- Web框架
	- Flask
tags:
---
requirements.txt，项目依赖关系清单，包含当前项目的依赖包名称及其对应的版本号。

## 1 生成
> pip freeze > requirements.txt

## 2 重建
> pip install -r requirements.txt

如果下载太慢，可以用豆瓣源下载
```
pip install -i https://pypi.douban.com/simple/ --trusted-host pypi.douban.com -r requirements.txt
```

