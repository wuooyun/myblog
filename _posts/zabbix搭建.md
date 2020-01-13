---
title: zabbix搭建
date: 2020-01-13 09:17:09
tags:
 - zabbix
categories:
 - zabbix
---

此文章仅记录整体搭建的流程

安装依赖 工具

安装数据库

安装zabbix

以上可参考zabbix文档此处不再赘述

zabbix创建监控主机
snmp 需要在*宏*处设置团体名   {SNMP_COMMUNITY} = public

创建监控项
1. 通过模板自动创建
2. 自定义创建item
需要找到对应的oid 例如华为官网会提供mib库查询 在linux下 用
snmpwalk -v 2c -c public 192.168.254.20 1.3.6.1.4.1.2011.5.25.31.1.1.1.1.5
找到带有数值的后缀 此例是创建cpu-usage监控的案例

[root@zb-zabbix ~]# snmpwalk -v 2c -c public 192.168.254.20 1.3.6.1.4.1.2011.5.25.31.1.1.1.1.5
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.3 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.262149 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.524293 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.786437 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.1048581 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.1310725 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.1572869 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.1835013 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.2097157 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.2359301 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.2359305 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.2621445 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.2883589 = INTEGER: 0
SNMPv2-SMI::enterprises.2011.5.25.31.1.1.1.1.5.2883593 = INTEGER: 9

例如本文就是1.3.6.1.4.1.2011.5.25.31.1.1.1.1.5.2883593
此oid作为cpu监控项的OID创建

### 创建触发器 
可根据item的返回值创建触发器

### 告警相关
1. 创建报警媒介类型
 支持自定义参数
  {ALERT.SENDTO} 发送的用户
  {ALERT.SUBJECT}  标题
  {ALERT.MESSAGE} 消息内容
2. 创建用户告警媒介
3. 创建动作
     此处可以传一些参数给脚本，交由脚本进行格式化、筛选、发送、展示、创建一些html等,我主要参考企业微信API,进行消息告警

