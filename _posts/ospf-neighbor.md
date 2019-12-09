---
title: OSPF邻居关系无法建立原因总结
date: 2019-03-24 17:23:10
tags:
 - OSPF
 - H3C
 - 计算机网络
---

| 参数 | 配置要点 |
| :----: | :---- |
| router id | 每台OSPF路由器的Router ID必须唯一 |
| area id | 同一网段的所有端口应当配置在同一区域内 |
| network mask | 除了点到点网络之外，同一网段的所有端口应当配置相同的掩码 |
| authentication type | 同一区域的验证类型必须一致 |
| authentication data | 同一区域的验证码必须一直 |
| extern option | 配置stub区域或者NSSA时，区域内所有的路由器都需要指定stub特性或者NSSA特性 |
| peer | NBMA网络上的邻居需要手动指定 |
