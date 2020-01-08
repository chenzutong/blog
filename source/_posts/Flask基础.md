---
title: Flask基础
date: 2019-12-02 09:29:14
categories:
	- Web框架
	- Flask
tags:
---
## 1 开启调试模式
如果启用了调试支持，服务器会在代码修改后自动重新载入，bing在发生错误时提供一个相当有用的调试器。
> app.debug = True
app.run()

或
> app.run(debug=True)

## 2 路由
客户端（如Web浏览器）把请求发送给Web服务器，Web服务器再把请求发送给Flask程序实例。程序实例需要知道对每个URL请求运行哪些代码，所以保存了一个URL到Python函数的映射关系。处理URL和函数之间关系的程序称为路由。

使用程序实例提供的app.route修饰器把修饰的函数注册为路由。如下：

	@app.route('/')
	def index():
		return '<h1>Hello World!</h1>h1>'

### 2.1 变量规则
要给URL添加变量部分，可以把这些特殊的字段标记为<variable_name>,这个部分将会作为命令参数传递到你的函数。

	@app.route('/user/<username>')
	def show_user_profile(username):
		return 'User %s' % username

![](user.png)

用<converter:variable_name>指定一个可选的转换器

	@app.route('/post/<int:post_id>')
	def show_post(post_id):
		return 'Post %d' % post_id

### 2.2 构造URL
用url_for()来指定的函数构造URL。

	from flask impost Flask, url_for

	@app.route('/url/')
	def get_url():
		return url_for('show_post', post_id=2)

![](url.png)

### 2.3 HTTP方法
HTTP（与Web应用会话的协议）有许多不同的访问URL方法。默认情况下，路由只回应GET请求，但是通过route()装饰器传递methods参数可以改变这个行为。

	from flask import Flask, url_for, request
	@app.route('/login', methods=['GET', 'POST'])
	def login():
	    if request.method == 'POST':
	        do_the_login()
	    else:
	        show_the_login_form()

HTTP方法告知服务器客户端相对请求的页面做些什么。
> GET:浏览器告知服务器：只获取页面上的信息并发给我。这是常用的方法。
> POST:浏览器告诉服务器：想在URL上发布新消息。并且，服务器必须确保数据已存储且仅存储一次。这是HTML表单通常发送数据到服务器的方法。
> HEAD:浏览器告诉服务器：欲获取信息，但是只关心信息头。应用应像处理GET请求一样来处理它，但是不分发实际内容。（底层的Werkzeug库处理）
> PUT:类似POST但是服务器可能触发了存储过程多次，多次覆盖掉旧值。
> DELETE:删除给定位置的信息。

## 3 静态文件
动态Web应用也会需要静态文件，通常是CSS和JavaScript文件。
在目录中创建一个名为staic文件夹，在应用中使用/static即可访问。
> url_for('static', filename='style.css')

## 4 蓝图（blueprints）
在一个应用中或跨应用组件和支持通用的模式。
> 创建  bp = Blueprint('admin', __name__, url_prefix='/admin')
> 注册  app.register_blueprint(bp)
