---
title: 'Flask简介'
date: 2019-12-01 11:03:40
categories:
	- Web框架
	- Flask
tags:
---
Flask依赖两个外部库：Werkzeug和Jinja2。
Werkzeug是一个WSGI工具集。
Jinja2负责渲染模板。

ps: 以下在windows系统操作
## 1 安装虚拟环境
目的：让各项目环境保持独立

### 1.1 安装Virtualenv
> pip install virtualenv

### 1.2 创建虚拟环境
> virtualenv venv

### 1.3 激活虚拟环境
可以用Tab键补进
> venv\Scripts\activate

退出虚拟环境
> deactivate

## 2 安装Flask
进入虚拟环境
> pip install flask

查看安装包
> pip list

### 3 第一个Flask程序
在venv同级目录下创建一个01.py文件

	from flask import Flask

	app = Flask(__name__)
	@app.route('/')
	def hello_world():
	    return 'Hello World'

	if __name__ == '__main__':
		app.run()

1.Flask类的实例会是我们的WSGI应用程序
2.创建该类的实例，第一个参数是应用模块或者包的名称。如果是单一的模块（本例），应该使用__name__,因为模块的名称将会因其作为单独应用启动还是作为模块导入而不同。
