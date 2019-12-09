---
title: H3C dhcp配置实例
date: 2018-09-30 11:28:34
categories:
 - H3C
tags:
 - dhcp
---

工作中用到的在**h3c-5500-v2**上配置**dhcp server**，留作日后复习参考。
***
#### 一、创建接口
```h3c
vlan 1 
int vlan 1
ip add 192.168.1.1 24
```
#### 二、dhcp相关配置
> * 创建地址池
```h3c
dhcp server ip-pool  pool-name #地址池名称
 gateway-list 192.168.1.1
 network 192.168.1.0 mask 255.255.255.0 #设置地址池网络
 dns-list 8.8.8.8 114.114.114.114 #设置DNS
 expired day 0 hour 8  #设置租期
 forbidden-ip 192.168.1.1 #设置当前地址池不分配的ip #不分配地址段须在全局配置
```
> * 应用dhcp地址池到到接口
``` h3c
 int vlan 1
 dhcp select server  #选择dhcp工作模式为服务端 ps：另一个为中继端
 dhcp server apply ip-pool pool-name #地址池名称
```
> * 开启dhcp服务
```h3c
dhcp enable
dhcp server forbidden-ip low-ip  up-ip 

```
#### 三、sub接口下dhcp

> * 启用DHCP服务。
``` h3c
  <SwitchA> system-view

  [SwitchA] dhcp enable
```
> * 配置VLAN接口10的主从IP地址，并配置该接口工作在DHCP服务器模式。
``` h3c
[SwitchA] interface vlan-interface 10

[SwitchA-Vlan-interface10] ip address 10.1.1.1 24

[SwitchA-Vlan-interface10] ip address 10.1.2.1 24 sub

[SwitchA-Vlan-interface10] dhcp select server

[SwitchA-Vlan-interface10] quit
```
> * 创建DHCP地址池aa，配置主网段地址范围和从网段地址范围，配置网关地址。
``` h3c
[SwitchA] dhcp server ip-pool aa

[SwitchA-dhcp-pool-aa] network 10.1.1.0 mask 255.255.255.0

[SwitchA-dhcp-pool-aa] gateway-list 10.1.1.254

[SwitchA-dhcp-pool-aa] network 10.1.2.0 mask 255.255.255.0 secondary

[SwitchA-dhcp-pool-aa-secondary] gateway-list 10.1.2.254

quit
```
