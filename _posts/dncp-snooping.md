---
title: dhcp-snooping
date: 2019-06-05 12:21:38
tags:
- DHCP
- DHCP安全
categories:
- 路由交换技术
---

### DHCP Snooping 
dhcp 窥探
作用: 防止非法的DHCP服务器

使用位置：
一般在接入层交换机启用
相关命令：
dhcp enable
dhcp snooping enable
vlan 8
dhcp snooping enable

int e0/0/2
dhcp snooping trusted 将上联接口设置为信任接口（默认情况下所有接口都是非信任接口）
设置此项以信任可信任的dhcp服务器



### 其他dhcp相关

HUAWEI 关于dncp信息查询
dis ip pool 查看dhcp池摘要

dis ip pool interface vlanif100 used 显示dhcp哪些地址被使用

reset ip pool interface vlanif100 used 重置DHCP分配记录


