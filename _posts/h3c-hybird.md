---
title: h3c-hybird
date: 2018-10-04 17:02:43
tags:
 - hybird端口
categories:
 - 华为
 - H3C
---

### 神魔是Hybird端口？

Access端口发往其他设备的报文，都是Untagged Frame，即不带802.1q tag的标准的以太网帧，而truck端口一般发出的都是Tagge Frame。在某些应用中，可能希望灵活控制Vlan标签的移除，例如在本交换机的上行设备不支持Vlan的情况下，希望实现各个用户端口相互隔离。Hybird端口可以解决此问题，它对接受不带Tag的报文处理同Access一致，对接受带Tag的报文处理同Trunk口一致。发送帧时，当Vlan ID是该端口允许通过的Vlan ID时，发送该报文，可以通过命令设置发送时是否携带Tag。

### Hybird端口常用用途例如

基于IP策略的VLAN定义 通过网络层协议或IP地址来确定虚拟网。交换机可根据各节点网络地址或报文协议自动将其划分成不同的VLAN。

