---
title: json
date: 2019-12-12 23:29:55
categories:
  - Python
  - 其他模块
tags:
---


# JSON数据解析
JSON(JavaScript Object Notation)是一种轻量级的数据交换格式。

> json.dumps():对数据进行编码
json.loads(): 对数据进行解码

## Python编码为JSON类型转换对应表

Pyhon | JSON
--- | ---
dict | object
lsit,tuple | array
str | string
int,float,int-&float-derived Enums | number
True | true
False | false
None | null

## JSON解码为Python类型转换对应表

JSON | Python
--- | ---
object | dict
array | list
string | str
number(int) | int
number(real) | float
true | True
false | False
null | None


## json.dumps与json.loads实例

```
import json
 
# Python 字典类型转换为 JSON 对象
data1 = {
    'no' : 1,
    'name' : 'Runoob',
    'url' : 'http://www.runoob.com'
}
 
json_str = json.dumps(data1)
print ("Python 原始数据：", repr(data1))
print ("JSON 对象：", json_str)
 
# 将 JSON 对象转换为 Python 字典
data2 = json.loads(json_str)
print ("data2['name']: ", data2['name'])
print ("data2['url']: ", data2['url'])
```
执行以上代码输出结果为：

> Python 原始数据： {'name': 'Runoob', 'no': 1, 'url': 'http://www.runoob.com'}<br>
JSON 对象： {"name": "Runoob", "no": 1, "url": "http://www.runoob.com"}<br>
data2['name']:  Runoob<br>
data2['url']:  http://www.runoob.com<br>

## json.dump()和json.load()
 如果处理的是文件而不是字符串，你可以使用 json.dump() 和 json.load() 来编码和解码JSON数据。
 
 ```
# 写入 JSON 数据
with open('data.json', 'w') as f:
    json.dump(data, f)
 
# 读取数据
with open('data.json', 'r') as f:
    data = json.load(f)
```