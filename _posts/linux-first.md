---
title: linux开局配置
date: 2019-11-29 17:30:56
tags:
 - linux网络配置
categories:
 - Linux
---


#### 前言
如何配置linux为一台可以正常上网的系统

### IP地址配置 
首先 ifConfig查看网络接口信息  
找到当前使用的接口一般为 eth0 或者 ens3

然后对该网口配置IP地址
sudo ifconfig eth0 192.168.1.1 netmask 255.255.255.0 up 
概念要搞清楚，ip和掩码是网络接口需要的，至于网关则是路由的东西了

### 路由配置
netstat -r 用来查看当前Linux的路由

路由的属性有 Destination(目的地址) Gateway(网关，下一跳) Genmask(网络掩码) Intface(接口) 接口对应的是当前接口流量的路由
添加路由
route add default gw 192.168.1.254 #添加默认路由
route add -net  10.1.1.0/24 gw 10.1.1.254    #-net参数用于添加一个网络路由
route add -host 10.1.1.1 gw 10.1.1.254 #-host参数用于添加一个主机路由

### dns配置
配置文件为 /etc/resolv.conf 
格式 
nameserver 223.5.5.5
nameserver 1.1.1.1



