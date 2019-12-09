---
title: 华三 NQA 配置命令简单介绍
date: 2018-10-04 14:19:19
tags:
 - nqa
categories:
 - H3C
---


### 记录对nqa配置部分命令解释
```
nqa entry admin hngd
   type icmp-echo #使用ICMP  ping的方式
   destination ip 10.114.10.1 #探测目的地址
   frequency 1000     #探测频率 单位ms  ps：1000ms=1s
   reaction 1 checked-element probe-fail threshold-type consecutive 5 action-typetrigger-only #
   ###行为1 连续探测失败5次后触发行为(此处与cisco sla 挂track后的delay有异曲同工之妙）
   reaction2 checked-element probe-failthreshold-type consecutive 10 action-type trigger-only #
   ###行为2 连续探测失败10次后触发行为
   ###Reaction  可以对NQA绑定多个行为，

nqa schedule adminhngd start-time now lifetime forever  #计划  现在开始，永远生效
track 3 nqa entry admin hnyd reaction1 追踪组 3   #绑定NQA hnyd  使用行为1

```


* 2019-3-24修改 *
