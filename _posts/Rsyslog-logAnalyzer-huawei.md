---
title: Rsyslog+logAnalyzer搭建日志主机
date: 2020-01-11 23:16:18
tags:
 - log
 - huawei
 - h3c
categories:
 - 路由交换技术
 - log
---
本次通过搭建日志服务中心对设备日志进行统一收集管理，以及web可视化

### 系统要求

mariaDB mariaDB-server  php php-mysql php-pdo php-common php-gd httpd

### 安装mysql

```shell
yum install mariadb mariadb-server -y
```
### 安装php
```shell
yum install php php-mysql php-pdo php-common php-gd httpd
```
### rsyslog+loganalyzer
Rsyslog使用centos7自带的8+版本
安装 yum install rsyslog-mysql
loganalyzer 下载地址：http://loganalyzer.adiscon.com/downloads

### 设置部分

初始化数据库 并创建用户rsyslog
```
cd /usr/share/doc/rsyslog-8.24.0/
mysql -uroot -p123456 < mysql-createDB.sql 
grant all on Syslog.* to rsyslog@localhost identified by '123456';
flush privileges;
```
配置rsyslog
```
vim /etc/rsyslog.conf
在 #### MODULES #### 下添加下面两行：
$ModLoad ommysql
*.*  :ommysql:localhost,Syslog,root,mysql
    #localhost字节是database-server
    #Syslog是数据中database-name
    #root
```

### 问题总结
本次搭建日志服务主要是h3c 和华为设备日志的格式和loganalyzer字段不匹配 
解决方法是在华三和华为设备中设置info-center的时间格式为short-time
还要就是loganalyzer初始化时，表选择为空




