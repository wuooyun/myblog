---
title: 关于cisco 4331单路由热备份上架的总结
date: 2018-08-12 17:10:22
type: "tags"
tags:
- 线路备份
- 路由交换技术
categories:
- 路由交换技术
---
### 关于单路由双isp备份的总结(sla+emm+router-map+acl)

[参考文档](https://blog.csdn.net/fhqmqj828007/article/details/6406023)

# 设备拓扑图
![](http://wx1.sinaimg.cn/mw690/0061nvdWgy1fu7sy1t95wj30g30dvmxt.jpg)

## 目的
* isp1为主链路，正常情况下所有流量走isp1，当isp1故障，流量自动切换至isp2
* 通过route-map+sla+emm实现自动快速切换，达到冗余备份目的

## 详细配置
# IP地址设置

ISP1（config）# int e0/0
ISP1 (config-if)  #ip add 1.1.1.1 255.255.255.0
ISP1config-if)  #no shutdown
***
ISP2（ config ） # int e0/0
ISP2 (config-if)  #ip add 2.2.2.1 255.255.255.0
ISP2(onfig-if)  #no shutdown 
***
R1 (config)  #int e0/0
R1 (config-if)  #ip add 1.1.1.2 255.255.255.0
R1 (config-if)  #no shutdown
R1 (config ） # int e0/1
R1 (config-if)  #ip add 2.2.2.2 255.255.255.0
R1 (config-if)  #no shutdown

# 定义相关ACL

R1(config)#ip access-list extended all-net …………………… 匹配所有
R1(config-ext-nacl)#permit ip any any
R1(config)#access-list 1 permit 1 1.1.1.1 ………… 匹配 ISP1 next-hop
R1(config)#access-list 2 permit 2 2.2.2.1 ………… 匹配 ISP2 next-hop

# Route-map、Nat

R1(config)#route-map isp1-line permit 10
R1(config-route-map)#match ip address all-net
R1(config-route-map )#match ip next-hop 1………………. 匹配 ACL 1 （关键）
R1(config)#route-map isp2-line permit 10
R1(config-route-map)#match ip address all-net
R1(config-route-map )#match ip next-hop 2………………. 匹配 ACL 2 （关键）

R1(config)#ip nat inside source route-map isp1-line int e0/0 overload
R1(config)#ip nat inside source route-map isp1-line int e0/1 overload

# IP sla、track

R1(config)#ip sla 1
R1(config-sla-monitor)# icmp-echo 1.1.1.1
R1(config-sla-monitor-echo)#frequency 5
R1(config)#ip sla schedule 1 life forever start-time now
R1(config)#track 1 ip sla 1 reachability

# 路由与nat出入口

R1 (config)#ip route 0.0.0.0 0.0.0.0 1.1.1.1 track 1
R1 (config)#ip route 0.0.0.0 0.0.0.0 36.33.24.1 100............优先级低
R1 (config) #int e0/0
R1 (config-if) #ip nat outside
R1 (config)  #int e0/1
R1 (config-if)  #ip nat outside
R1 (config) #int e0/2
R1 (config-if) #ip nat inside
R1 (config)  #int e0/3
R1 (config-if)  #ip nat inside

# cisco EEM清除nat缓存 加快链路收敛

R1 (config)#event manager applet ISP1DOWN
 event syslog occurs 1 pattern "111 ip sla 1 reachability Up -> Down"
 action 1 cli command "enable"
 action 2 cli command "clear ip nat trans *"
 action 3 cli command "end"
R1 (config)#event manager applet ISP1UP
 event syslog occurs 2 pattern "111 ip sla 1 reachability Down -> Up"
 action 1 cli command "enable"
 action 2 cli command "clear ip nat trans *"
 action 3 cli command "end"

# 此次上架出现的问题
第一次因为acl配置问题
第二次路由器流量达不到100M带宽，刚开始带宽30M，后来带宽一直8M以内，上架后未检测网速，导致业务卡顿。
第三次排查原因，联想到之前升级过ios版本，可能存在ios版本不兼容的问题。退回之前版本，测试正常。
# 2018.10.16在网络环境波动的情况下和网络负载较大的情况下SLA+track翻动问题
问题一、此次由于sla icmp ping包出现频繁掉包情况，导致track频繁的UP Down 主备链路频繁切换，导致网络震荡
问题二、此次cisco  EEM appelt ispup 未执行清除nat缓存，导致网络延时。
## 解决方案：
#### 一、降低sla探测敏感度  在track中增加down的时延具体配置：
 Router(config)#track 1 ip sla 1 reachability 
 Router(config-track)#delay down 30    -----down计时器，30s如果sla icmp都失败，则track生效，若30s内有icmp成功，则重置计时器。
#### 二 、 EEM问题未知但现在改用一下方式正常
R1 (config)#event manager applet ISP1UP
Router(config-applet)#event track 1 state up 
 action 1 cli command "enable"
 action 2 cli command "clear ip nat trans *"
 action 3 cli command "end"







