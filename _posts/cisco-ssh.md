---
title: 思科路由器配置SSH安全实现远程管理
date: 2018-10-02 17:51:54
tags:
 - ssh
---

### 1、 配置hostname和ip domain-name

SW#configure terminal
SW(config)#hostname R2      //配置ssh的时候路由器的名字不能为router
SW(config)#ip domain-name cisco.com    //配置SSH必需 
SW(config)aaa new-model 这个默认是开启的不用打也行，但要看是否有 no aaa new-model 如果no掉了ssh就无法认证了 
SW(config)#username admin privilege 15 secret passwd 建议一个登陆用户
SW(config)#line vty 0 15SW(config-line)#transport input ssh //只允许用SSH登录(注意：禁止telnet和从交换引擎session!)

SW(config-line)#transport input ssh telnet //这样就又允许ssh又允许telet 如果这样是很危险的，可以用Access进行一下限制，某些IP段可以Telnet某具必须ssh

******

AA的SSH配置：
ip domain-name runway.cn.net-------------------------------------------设置域名
aaa new-modle----------------------------------------------------------启用AAA服务
crypto key generate rsa------------------------------------------------生成秘钥
The name for the keys will be: Router1.runway.cn.net
Choose the size of the key modulus in the range of 360 to 2048 for your
General Purpose Keys. Choosing a key modulus greater than 512 may take
a few minutes.
How many bits in the modulus [512]: 1024-------------------------------指定1024位秘钥
% Generating 1024 bit RSA keys ...[OK]
username sshuser secret sshpassword------------------------------------指定SSH登陆用户名和密码
ip ssh time-out 30-----------------------------------------------------设定SSH超时值
no ip ssh version------------------------------------------------------启用SSH V1 V2
aaa authentication login ssh local line none---------------------------设定SSH登陆信息存储地方
ip ssh authentication-retries 3 ---------------------------SSH登陆三次后关闭当前连接
ip access-list standard forssh-----------------------------------------定义SSH登陆源地址
permit any

line vty 0 4
exec-timeout 30------------------------------------------------------设置线路登陆超时值
login authentication ssh---------------------------------------------指定验证登陆用户信息存储的地方
transport input ssh--------------------------------------------------设置线路登陆模式为SSH
access-class forssh in-----------------------------------------------应用访问列表
--------------------- 
作者：galdys 
来源：CSDN 
原文：https://blog.csdn.net/Galdys/article/details/6590717?utm_source=copy 
版权声明：本文为博主原创文章，转载请附上博文链接！
