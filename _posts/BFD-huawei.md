---
title: BFD双向链路检测
date: 2019-06-06 12:01:41
tags:
 - BFD
 - 链路质量检测
categories:
 - 路由交换技术
---

### BFD双向链路检测

BFD: Bidirectional Forwarding Detection,双向转发检查

作用: 毫秒级故障检查，通常结合三层协议(如静态路由、VRRP、OSPF、BGP等)实现链路故障快速检查和切换。

#### 静态路由调用BFD
R1：
IP router-static 2.2.2.0 255.255.255.0 12.1.1.2 preference 50 将路由优先级改为50 设置为优选路径
ip route-static 2.2.2.0 255.255.255.0 21.1.1.2

配置BFD:
R1:
bfd 全局使能bfd
quit
bfd 1 bind peer-ip 12.1.1.2 source-ip 12.1.1.1
discriminator local 1 本地标识 两台路由器的标识需要匹配
discriminator remote 2  远端标识
commit 确认提交配置

ip route-static 2.2.2.0 255.255.255.0 12.1.1.2 preference 50 bfd-sesssion 1

R2:
对称配置即可

查看BFD信息
dis bfd session static
dis bfd session static verbose

#### 动态BFD: ospf调用BFD加快收敛

R1：
ospf 1
bfd all-interfaces enable 对于所有位于ospf的接口全部企业bfd
area 0.0.0.0
network 12.1.1.0 0.0.0.255
network 1.1.1.0 0.0.0.255

查看BFD信息
dis bfd session dynamic [verbose]

#### VRRP联动BFD:通常vrrp上联有二层设备隔离，此时可以借助BFD实现故障检测。

int e0/0/0
  vrrp vrid 10 track bfd-session 100 vrrp调用BFD  100 是BFD的本地(local)标识

#### VRRP调用BFD调用动态BFD的会话名称

注：vrrp 调用BFD的会话名称，要求BFD必须为动态 
R1:
bfd bb bind peer-ip 14.0.0.4 source-ip 14.0.0.1 auto 创建动态BFD bb 
int e0/0/0 
   vrrp vrid 125 track bfd-session session-name bb 

