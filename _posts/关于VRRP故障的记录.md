---
title: 关于VRRP故障的记录
date: 2019-06-16 19:00:43
tags:
- VRRP
categories:
- 路由交换技术
---
### 网络环境
 
 vrrp + mstp + ospf

### 关于故障排查的一些要点

1. 流量转发的方式

同一网段，查mac地址表，若存在对应mac则转发，若不存在对应ip则进行arp请求(广播)
不通网段，将浏览转发到网关，由网关设备检查ip层查找路由表 进行转发

2. 日志文件
h3c display logbuffer
    more flash:/log/log,file
### 
3. 分层思想

参考OSI七层模型

4. 针对不同协议分析

vrrp心跳报文默认走二层设备 

preempt delay   100
vrrp 抢占时延单位厘秒 1s=100厘秒

