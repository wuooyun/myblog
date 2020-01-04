---
title: NAT相关整理
date: 2019-12-30 16:01:16
tags:
- nat
- huawei
categories:
- 路由交换技术
---

nat技术是一个应用极为广泛的技术，也是一个网工必须掌握的一项技术。

nat一般应用场景

这里提到的NAT均已pat的nat，带有端口转换
1 内网用户访问公网
一个出口ip
华为的配置
1、acl匹配要通过nat的流量
2、方式一、 接口配置公网ip，nat outbount aclnumber #这种方式为easyip方式
方式二、创建nat地址池
nat address-group groupnumber ipaddress_start ipaddress_end
这里有几点需要注意的 1、地址池地址不能和接口地址相同.2、单地址池地址连续
emmm这里要吐槽下，如果两个出口ip在出接口上我是用地址池还是easyip呢？
nat 会对子接口ip进行nat转换吗？

2 内网服务器对公网提供服务
 直接nat server
 

ps 草记下主要给自己看，其他人看到这么乱不要吐槽我，哈哈— 。—
