---
title: Linux黑洞路由
date: 2018-12-02 17:50:54
tags:
- Linux
- Linux安全
- 黑洞路由
categories:
- Linux
---

Linux受到固定ip或者其他攻击,简单想到的办法是iptable,比如
```bash
iptable -A INPUT -s IP -j DROP
````
实际上,超大规模的攻击使用iptable过滤会非常耗CPU,一般建议使用黑洞路由

从这个网址: http://www.cyberciti.biz/tips/how-do-i-drop-or-block-attackers-ip-with-null-routes.html 可以看到

假设需要屏蔽掉202.33.8.49,有三种不同做法:
```bash
1. # route add 202.33.8.49 gw 127.0.0.1 lo

2.#ip route add blackhole 202.33.8.49

3.#route add -host 202.33.8.49 reject
```
当然,文中没有解释这三种做法的区别与优劣,这里简单说说个人看法

第一种,是把这个ip的路由导向lo 127.0.0.1,其实是相当犯傻的行为,系统会自动的尝试往lo发送数据(tcpdump可见)

第二种,是加入黑洞路由,意思就是直接就丢弃了,当然也不会有对应的应用层程序收到数据包尝试回包了

第三种,先看看man里边怎么说: “reject install a blocking route, which will force a route lookup to fail.  This is for example used to mask out networks before using the default route.  This is NOT for firewalling.”  reject是阻止了”到”这个IP的网络,通常用于在使用默认路由前标识网络,不适用于防火墙,为什么呢?因为应用层程序能收到数据包,只不过尝试回包时会知道网络不可达而已

