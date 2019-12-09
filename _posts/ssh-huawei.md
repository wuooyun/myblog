---
title: 华为ssh配置
date: 2019-01-02 16:57:25
tags:
 - ssh
categories:
 - 华为
---
### 1.在服务器端生成本地密钥对
<huawei>system-view 
[HUAWEI] sysname SSH_Server 
[SSH_Server] dsa local-key-pair create 
Info: The key name will be: HUAWEI_Host_DSA. 
Info: The key modulus can be any one of the following : 1024, 2048. 
Info: If the key modulus is greater than 512, it may take a few minutes.        
Please input the modulus [default=2048]: 
Info: Generating keys... 
Info: Succeeded in creating the DSA host keys.</huawei>
### 2.在服务器端创建SSH用户
#### a.配置VTY用户界面。
[SSH_Server] user-interface vty 0 14 
[SSH_Server-ui-vty0-14] authentication-mode aaa
[SSH_Server-ui-vty0-14] protocol inbound ssh
[SSH_Server-ui-vty0-14] quit
#### b.新建用户名为client001的SSH用户，且认证方式为Password。
[SSH_Server] aaa 
[SSH_Server-aaa] local-user client001 password irreversible-cipher Huawei@123 
[SSH_Server-aaa] local-user client001 privilege level 3
[SSH_Server-aaa] local-user client001 service-type ssh
[SSH_Server-aaa] quit 
[SSH_Server] ssh user client001 authentication-type password
### 3.SSH服务器端开启STelnet服务功能
[SSH_Server] stelnet server enable
### 4.配置SSH用户client001的服务方式为STelnet  
[SSH_Server] ssh user client001 service-type stelnet
### 5.客户端登录
