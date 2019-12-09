---
title: 统一管理三层ip资源之SuperVlan的应用
date: 2019-09-09 15:45:56
tags:
 - superVlan
categories:
 - 路由交换技术
---
### 背景
由于公司目前甲方分配ip资源缺少，公司按照职场划分vlan，那么统一管理ip资源可以较好的解决目前三层ip子网划分，单独vlan单独管理的问题。

### 华为三层交换机实现
#### 配置sub-vlan：

Sub-VLAN可以加入物理接口，但不能创建对应的VLANIF接口，所有Sub-VLAN内的接口共用Super-VLAN的VLANIF接口IP地址
[Switch] interface gigabitethernet 0/0/1 //0/0/2,0/0/3，0/0/4配置类似
[Switch-GigabitEthernet0/0/1] port link-type access //接口类型为access
[Switch-GigabitEthernet0/0/1] quit

[Switch] vlan 2
[Switch-vlan2] port gigabitethernet 0/0/1 0/0/2
[Switch-vlan2] quit

[Switch] vlan 3
[Switch-vlan3] port gigabitethernet 0/0/3 0/0/4 [Switch-vlan3] quit

### 配置super-VLAN。

[Switch] vlan 4
[Switch-vlan4] aggregate-vlan
[Switch-vlan4] access-vlan 2 to 3
[Switch-vlan4] quit

#### 配置VLANIF。

[Switch] interface vlanif 4
[Switch-Vlanif4] ip address 10.1.1.12 255.255.255.0
[Switch-Vlanif4] quit
### 注意事项
通过undo int vlan vlanid来取消三层vlan
