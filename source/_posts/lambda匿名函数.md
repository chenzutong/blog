---
title: lambda,map(),reduce()函数
date: 2019-12-16 22:19:00
categories:
  - Python
  - 进阶篇
tags:
---
## lambda匿名函数

lambda函数的语法值包含一个语句。其语法：
```
lambda [arg1 [,arg2,...argn]]: expression
```

* 冒号(:)之前的arg1,arg2,...表示函数的参数
* 匿名函数不需要return来返回值，表达式本身的结果就是返回值

示例1：
```
sayHello = lambda: print("hello , Python")
sayHello()
```

输出
> hello , Python

示例2：
```
sum = lambda arg1, arg2: arg1 + arg2
print(sum(10, 20))
```

输出
> 30

## map()函数

根据提供的函数对指定序列做映射，第一个参数function是一个函数对序列中每个元素都调用function函数，返回包含每次function函数返回值的新序列。语法：
```
map(function, iterable)
```
* function: 是个函数
* iterable: 一个或多个序列，也被称为可迭代对象

示例1：
```
def fun(x):
	return x * x
result = map(fun, [1,2,3])
print(list(result))

输出：
[1, 4, 9]
```

示例2：
```
result = map((lambda x: x + 3), [1,3,5,6])
print(list(result))

输出
[4, 6, 8, 9]
```
示例3：
```
result = map((lambda x,y: x+y), [1,2,3], [6,7,9])
print(list(result))

输出
[7, 9, 12]
```

## reduce()函数
对参数序列中的元素进行积累。语法：
```
reduce(function, iterable)
```
* function: 接收函数，必须有两个参数
* iterable: 一个或多个序列，序列也称为可迭代对象

reduce()函数在python3中被定义在functools包中  
示例1：
```
from functools import reduce
def add(x, y):
	return x + y
data = [1,2,3,4]
r = reduce(add, data)
print(r)

输出
10
```