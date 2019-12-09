---
title: ssh-sec
date: 2019-01-10 09:24:18
tags:
 - Linux安全
categories:
 - Linux
---
本diao的服务器老是被一些ip扫描，试图暴力破解ssh服务，不胜其扰，为了安全起见，用尽所学，决定
写上一个ssh自动拉黑相关ip的shell脚本。
参考一些文章后 脚本如下：
```bash
#!/bin/bash

 hosts_deny=/etc/hosts.deny

 hosts_secure=/etc/hosts.secure

 secure(){
 lastb | awk -F " " '{print $3}' | uniq -c |sort -nr | while read line 
   do
     attack_count=`echo $line | awk '{print $1}'` #ssh登录失败次数
     attack_ip=`echo $line | awk '{print $2}'` #ssh登录失败IP
     if [ "$attack_count" -gt "5" ] #如果失败次数大于5 则将deny语句暂存到/etc/hosts.secure 后面经筛选再加入hosts.deny中
     then
	echo sshd:"$attack_ip":deny >> $hosts_secure
     fi
   done	
 }
 secure
 sort $hosts_deny $hosts_secure | uniq > $hosts_deny #筛除重复ip，并写入配置
 logpath=/var/log/ssh_login_err.log
 log(){
      if [ -e $logpath ]
      then
         touch "$logpath"
      fi

      lastb | awk -F " " '{print $3}' | uniq -c |sort -nr  >> $logpath #留个日志i
      date >> $logpath
      echo "_______________" >> $logpath
 }

```
日志文件存放在/var/log/ssh_login_err.log中
后面将脚本加入到crontab中 即可根据设定的时间自动扫描拉黑登录失败5次以上的ip
