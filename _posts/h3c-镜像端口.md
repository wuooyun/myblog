---
title: h3c-镜像端口
date: 2018-11-05 09:19:55
tags:
 - 镜像端口
categories:
 - H3C
---

### 配置镜像端口，用于抓包排错
 <H3C>system-view
vlan 999
int g1/0/24
port acc vlan 999   创建空闲vlan
[H3C]mirroring-group 1 local     建立本地镜像组
[H3C]mirroring-group 1 mirroring-port g1/0/23    配置源端口即被镜像端口
[H3C]mirroring-group 1 monitor-port g1/0/24     配置目的端口（原闲置端口）

配置完成后，需要针对网络进行抓包分析时，从镜像端口可以抓到核心交换机下所有的和外网相关的数据包。
