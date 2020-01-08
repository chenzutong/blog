---
title: sqlite3
date: 2019-12-12 23:27:37
categories:
  - Python
  - 其他模块
tags:
---



## 连接数据库

```
import sqlite3
conn = sqlite3.connect("test.db")
```
>sqlite3.connect(database [,timeout ,other optional arguments])<br>
连接到一个现有的数据库。如果数据库不存在，那么它就会被创建，最后将返回一个数据库对象。

>connect.close()<br>
关闭数据库连接。

## 创建表

```
import sqlite3
conn = sqlite3.connect("test.db")
c = conn.cursor()
c.execute(
    '''
    CREATE TABLE COMPANY
    (ID INT PRIMARY KEY NOT NULL,
    NAME TEXT BOT NULL,
    AGE INT NOT NULL,
    ADDRESS CHAR(50),
    SALARY REAL);
    '''
)
conn.commit()
conn.close()
```
>connect.cursor([cursorClass])<br>
创建一个 cursor，将在 Python 数据库编程中用到。

>cursor.execute(sql [, optional parameters])<br>
该例程执行一个 SQL 语句。

>connect.commit()<br>
提交当前的事务

## INSERT操作

```
import sqlite3
conn = sqlite3.connect("test.db")
c = conn.cursor()
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)"
          " VALUES (1, 'Paul', 32, 'California', 20000.00)")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)"
          " VALUES (2, 'Allen', 25, 'Texas', 15000.00)")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)"
          " VALUES (3, 'Teddy', 23, 'Norway', 20000.00 )")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)"
          " VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00)")
conn.commit()
conn.close()
```

## UPDATE操作

```
import sqlite3
conn = sqlite3.connect('test.db')
c = conn.cursor()
c.execute("UPDATE COMPANY set SALARY = 25000.00 where ID=1")
conn.commit()
print("Total number of rows updated :", conn.total_changes)
cursor = c.execute("SELECT id, name, address, salary  from COMPANY") # 是一个生成器
for row in cursor:
    print("ID = ", row[0])
    print("NAME = ", row[1])
    print("ADDRESS = ", row[2])
    print("SALARY = ", row[3], "\n")
conn.close()
```

> connection.total_changes()<br>
返回自数据库连接打开以来被修改、插入或删除的数据库总行数。

## DELETE操作

```
import sqlite3
conn = sqlite3.connect('test.db')
c = conn.cursor()
c.execute("DELETE from COMPANY where ID=2;")
conn.commit()
print("Total number of rows deleted :", conn.total_changes)
cursor = c.execute("SELECT id, name, address, salary  from COMPANY")
print(cursor)
for row in cursor:
    print("ID = ", row[0])
    print("NAME = ", row[1])
    print("ADDRESS = ", row[2])
    print("SALARY = ", row[3], "\n")
conn.close()
```







