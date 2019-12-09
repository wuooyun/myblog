---
title: mysql应用
date: 2019-11-14 17:30:06
tags:
 - mysql
---
初始登录
mariadb 应该可以不用passwd

mysql -u root  或者 mysql进入mysql内部视图
或者 mysql -u root -p     回车 设置初次密码


创建数据库 表
create database 数据库名；
使用指定数据库
use 数据库名

将数据库所有权限授予mysql用户
GRANT ALL ON 数据库名.* TO 'your_mysql_name'@'your_client_host';
其中your_mysql_name是分配给您的MySQL用户名，your_client_host是连接服务器的主机。


创建一个数据库用户 z1 密码 ‘123’，具有对 sakila 数据库中所有表的 SELECT/INSERT 权限：
```sql
mysql> grant select,insert on sakila.* to 'z1'@'localhost' identified by '123';
```
