---
title: 组播协议应用
date: 2019-05-15 16:51:46
tags:
 - IGMP
 - 组播
categories:
 - 路由交换技术
---

### 组播协议

通常，我们把工作在网络层的IP组播称为“三层组播”，相应的组播协议成为“三层组播协议”，包括IGMP、PIM、MSDP、MBGP等；把工作在数据链路层的IP组播成为”二层组播“，相应的组播协议称为”二层组播协议“，包括I，包括IGMP Snooping、组播VLAN等。

#### 三层组播协议
三层组播协议包括组播组管理协议和组播路由协议两种类型，它们在网络中的应用位置如下图：

![三层组播协议应用位置](https://s5.51cto.com/wyfs01/M02/32/72/wKioOVKBihaArFLqAAApmLII3mg253.jpg "三层组播协议应用位置")
1. 组播组管理协议

在主机和与其直接相连的三层设备之间通常采用组播组的管理协议IGMP(Internet Group Management Protocol),该协议规定了主机与三层设备之间建立和维护组播组成员关系的机制。

2. 组播路由协议

组播路由协议运行在三层组播设备之间，用于建立和维护组播路由，并正确、高效地转发组播数据包。组播路由建立了从一个数据源端到多个接收端的无环(loop-free)数据传输路径、即组播分发树。

对于ASM（Any-Source Multicast，任意信源组播），可以将组播路由分为域内和域间两大类：

* 域内组播路由用来在AS内部发现组播源并构建组播分发树，从而将组播信息传递到接收者。在众多域内组播协议中，PIM(Protocol  Independent Multicast，协议无关组播)是目前较为典型的一个。按照转发机制的不同，PIM 可以分为DM(Dense   Mode，密集模式)和SM(Sparse Mode，稀疏模式)两种模式。
* 域间组播路由用来实现组播信息在AS 之间的传递，目前比较成型的解决方案有：MSDP(Multicast Source Discovery  Protocol，组播源发现协议)能够跨越AS 传播组播源的信息;而MP-BGP(MultiProtocol Border Gateway  Protocol，多协议边界网关协议)的组播扩展MBGP(Multicast BGP)则能够跨越AS 传播组播路由。

对于SSM 模型，没有域内和域间的划分。由于接收者预先知道组播源的具体位置，因此只需要借助PIM-SM 构建的通道即可实现组播信息的传输。

#### 二层组播协议
二层组播协议包括IGMP Snooping和组播VLAN等，它们在网络中的应用位置如下图。

![二层组播协议的应用位置](https://s2.51cto.com/wyfs01/M02/32/72/wKioOVKBipXy6JqHAAAcmsZFB2s984.jpg)

1. IGMP Snooping

IGMP Snooping(Internet Group Management Protocol   Snooping，互联网组管理协议窥探)是运行在二层设备上的组播约束机制，通过窥探和分析主机与三层组播设备之间交互的IGMP   报文来管理和控制组播组，从而可以有效抑制组播数据在二层网络中的扩散。

2. 组播VLAN

在传统的组播点播方式下，当连接在二层设备上、属于不同VLAN 的用户分别进行组播点播时，三层组播设备需要向该二层设备的每个VLAN   分别发送一份组播数据;而当二层设备运行了组播VLAN 之后，三层组播设备只需向该二层设备的组播VLAN   发送一份组播数据即可，从而既避免了带宽的浪费，也减轻了三层组播设备的负担。


[文章部分摘自51CTO博客 原文写的很清晰 点我可达](https://blog.51cto.com/12633577/1977993)





