数据库定义语言

库和表的管理

创建：create

修改：alter

删除：drop

### 库的管理

库的创建

语法：

​		create database [ IF NOT EXISTS] 库名；



库的修改

​	废弃



库的删除

DROP DATABASE [ IF EXISTS ] books;



### 表的管理

表的创建

​	create table 表名（

​				列名 列的类型 【（长度） 约束】，

​				列名 列的类型 【（长度） 约束】，

​				列名 列的类型 【（长度） 约束】，	

​				......

​				列名 列的类型 【（长度） 约束】

）



表的修改

​	alter table 表名 add|drop|modify|change column 列名 【列类型 约束】



​	修改列名

​		alter table book change column name1 name2 varchar;

​	修改列的类型或约束

​		alter table book modify column name int

​	添加新列

​		alter table book add column time date;

​	删除列

​		alter table book drop column name

​	修改表名

​		alter table book rename to book_FF



表的删除

​	drop table [ IF EXISTS ] book;



通用写法：

​		DROP DATABASE IF EXISTS 旧库名；

​		CREATE DATABASE 新库名；



​		DROP TABLE IF EXISTS 旧表名；

​		CREATE TABLE 表名();



表的复制

1.仅仅复制表的结构

​	create table copy like book;

2.复制表的结构+数据

​	create table copy

​		select * from book;

​	只复制部分数据

​	create table copy

​		select id,name

​		from book

​		where time = ' ' ;

















