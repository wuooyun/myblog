---
title: 利用Graylog搭建日志服务主机
date: 2020-01-04 19:37:45
tags:
 - logserver
 - log
categories:
 - Linux
 - Ops
---
### 背景

为了对各种网络设备的日志进行统一管理，现准备着手搭建一个日志服务主机。

### 实施

#### 利用到的开源工具
linux -Ubuntu18.04 --操作系统
Graylog            --日志分析工具
贴个Graylog文档的地址 https://docs.graylog.org/

#### 安装方法
emmm 文档里写的比我说的清楚
这里仅记录下踩过的坑

1、国内按照文档的方法来搞下载所需的包极其缓慢，这里最好挂下代理
2、安装好之后Graylog的配套工具 elasticserach无法启动，查看了下报错为内存溢出，elasticsearh和Graylog都是内存大户啊，两个跑起来吃了我大概4G的内存。当然可以根据实际的内存使用情况通过对jvm的设置 可以调整下内存占用 文章见 https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html


