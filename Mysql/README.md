# 这里是记录了MySQL的操作命令

## 链接MySQL

> mysql -h101.101.101.101 -uroot -pabcd123

## 修改密码

> mysqladmin -uroot -pab12 password djg345

## 显示命令

- 显示数据路列表

> show databases;

- 显示库中的数据表

> use mysql;

> show tables;

- 显示数据表的结构

> describe 表名;

- 建库

> create database 库名;

- 建表

> use 库名;

> create table 表名(字段设定列表);

- 删库和删表

> drop database 库名;

> drop table 表名;

- 将表中记录清空

> delete from 表名;

- 显示表中的记录

> select * from 表名;