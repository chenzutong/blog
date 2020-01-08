---
title: MySQL
date: 2019-12-01 09:16:28
categories:
	- MySQL
tags:
---
### MySQL连接
> [root@host]# mysql -u root -p
Enter password:

### 退出
> mysql> exit
Bye

### 创建数据库
> CREATE DATABASE 数据库名;

### 删除数据库
> drop database <数据库名>;

### 选择数据库
> mysql> use RUNOOB;
Database changed
mysql>

### 创建数据表
> CREATE TABLE table_name (column_name column_type);

如在 RUNOOB 数据库中创建数据表runoob_tbl：
> mysql> CREATE TABLE runoob_tbl(
   -> runoob_id INT NOT NULL AUTO_INCREMENT,
   -> runoob_title VARCHAR(100) NOT NULL,
   -> runoob_author VARCHAR(40) NOT NULL,
   -> submission_date DATE,
   -> PRIMARY KEY ( runoob_id )
   -> )ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.16 sec)
mysql>

### 删除数据表
> DROP TABLE table_name ;