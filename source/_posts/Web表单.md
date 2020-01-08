---
title: Web表单
date: 2019-12-03 19:26:41
categories:
	- Web框架
	- Flask
tags:
---
表单是允许用户跟Web应用交互的基本元素。
Flask-WTF插件允许用户在Flask应用中使用著名的WTFroms包。
安装：
> pip install flask-wtf

## 1 CSRF保护和验证
CSRF(cross site request forgery),跨站请求伪造。
Flask-WTF能保护所有表单免受跨站请求伪造的攻击。
Flask-WTF需要程序设置一个密钥。这个密钥生成加密令牌，再用令牌验证请求中表单数据的真伪。
> app = Flask(__name__)
app.config['SECRET_KEY'] = 'zutong'

## 2 表单类
使用Flask-WTF时，每个Web表单都由一个继承自Form的类表示。这个类定义表单中的一组字段，每个字段都用对象表示。
字段对象可附属一个或多个验证函数。验证函数用来验证用户提交的输入值是否符合要求。
如：创建一个包含一个文本字段、密码字段和一个提交按钮的简单Web表单：

	from flask_wtf import FlaskForm
	from wtforms import StringField, PasswordField, SubmitField
	from wtforms.validators import DataRequired


	class NameForm(FlaskForm):
	    name = StringField('请输入姓名', validators=[DataRequired()])
	    password = PasswordField('请输入密码', validator=[DataRequired()])
	    submit = SubmitField('Submit')

WTForms支持的HTML标准字段
> StringField: 文本字段
TextAreaField: 多行文本字段
PasswoerField: 密码文本字段
HiddenField: 隐藏文本字段
DateField: 文本字段，值为datetime.date格式，代表xxxx年xx月xx日   只表示前面的日期
DateTimeField: 文本字段，值为datetime.datetime格式，代表xxxx年xx月xx日xx时xx分xx秒，精确到时分秒,用于做时间戳
IntegerField: 文本字段，值为整数
DecimalField: 文本字段，值为decimal.Decimal
FloatField: 文本字段，值为浮点数
BooleanField: 复选框，值为True和False
RadioField: 一组单选框
SelectField: 下拉列表
SelectMultipleField: 下拉列表，可选择多个值
FileField: 文件上传字段
SubmitField: 表单提交按钮
FormField: 把表单作为字段嵌入另一个表单
FieldList: 一组指定类型的字段

WTForms内置验证函数
> Email: 验证电子邮件地址
EqualTo: 比较两个字段的值；常用于要求输入两次密码进行确认的情况
IPAddress: 验证IPv4网络地址
Length: 验证输入字符串的长度
NumberRange: 验证输入的值在数字范围内
Optional: 无输入值时跳过其他验证函数
DataRequired: 确保字段中有数据
Regexp: 使用正则表达式验证输入值
URL: 验证URL
AnyOf: 确保输入值在可选值列表中

## 3 把表单渲染成HTML
表单字段是可调用的，在模板中调用后会渲染成HTML。
例子解释：创建main.py

	from flask import Flask, render_template
	from flask_wtf import FlaskForm
	from wtforms import StringField, PasswordField, SubmitField
	from wtforms.validators import DataRequired


	class LoginForm(FlaskForm):
	    name = StringField(label='用户名', validators=[DataRequired('用户名不能为空')])
	    password = PasswordField(label='密码', validators=[DataRequired('密码不能为空')])
	    submit = SubmitField(label='提交')
	    
	    
	app = Flask(__name__)
	app.config['SECRET_KEY'] = 'zutong'


	@app.route('/', methods=['GET','POST'])
	def index():
	    form = LoginForm
	    data = {}
	    if form.validate_on_submit():
	        data['name'] = form.name.data
	        data['password'] = form.password.data
	    return render_template('index.html', form=form, data=data)


	if __name__ == '__main__':
	    app.run(debug=True)

templates\index.html


![](index.png)

> python main.py
