---
title: Windows域搭建及管理
date: 2019-01-24 23:32:35
tags:
- windows
- AD域
categories:
 - windows
---
由于终端安全的要求，公司决定加域控，进行统一管理。在此做些总结。
### 我的系统
服务端操作系统：Windows Server 2008 R2
终端系统：windows 7 旗舰版
### 域的搭建
服务端建立域较为简单，可以参考 https://jingyan.baidu.com/article/19192ad8e1593ae53e5707be.html
#### 需要注意的点：
> * *域名*：域名格式 一般*公司名.com*
> * *DNS*：服务器dns设置成127.0.0.1或者本机ip ，ip需要固定

### 域的管理
####需求目录： 
1. 统一域用户桌面背景
2. 域用户屏幕保护，
3. 密码复杂度，
4. 禁用usb存储,
5. 禁止安装软件。
6. 域成员400多人，批处理创建用户，管理用户
#### 统一域用户桌面背景
[参照](http://blog.51cto.com/itzhongxin/1918291)

#### 域用户屏幕保护
[参照](http://blog.chinaunix.net/uid-26184465-id-3516248.html)
注意点：.scr程序可以在 %systemroot%/system32/ 下面搜索 
相关脚本
```bat
IF NOT EXIST C:\Scr (goto 0) ELSE GOTO 1
:0
mkdir C:\Scr
xcopy \\192.168.254.2\meituanIP\历史软件\图标\* C:\Scr\ /S /E /H
goto :eof

:1
goto :eof
```
#### 批处理创建用户
[参照](https://blog.csdn.net/afterain/article/details/7406058)
注意事项
本脚本加了一些拓展
user
cn=用户名 ou=OU 
> -samid 登陆名
> upn 用户主体名称 (UPN) 组成用户帐户登录名和用户主要名称后缀加入由"@"符号。UPN 可以简化登录。通常情况下，UPN 是用户的电子邮件地址。因为用户主要名称提供了能够在企业中的任意位置执行单个登录，UPN 才能在整个 Microsoft Windows 2000 林中是唯一的。
> -ln LastName
  指定要添加的用户的姓氏。
> -fn FirstName
  指定要添加的用户的名字。
> -memberof GroupDN ...
  指定希望用户加入的组的可分辨名称。
  cn=组名 ou=OU dc=域前半部分 dc=域后半部分
> -pwd {Password | *}
  指定将用户密码设置为 Password 或 *。如果设置为 *，将提示您输入用户密码。
更多参数见 https://blog.csdn.net/zdbfba739/article/details/27484847

相关脚本
```bat
for /f "tokens=1,2,3,4,5 delims=," %a in (c:\AD.csv) do dsadd user "cn=%c,ou=SY3,DC=sanyou,dc=com" -samid %d -upn %d@sanyou.com -ln %a -fn %b -pwd %e -memberof cn=酒店在线,ou=SY3,dc=sanyou,dc=com -disabled no
```
#### 漫游用户配置文件
每个用户分配不同文件夹
#### 禁止IP修改
组策略编辑器》计算机》安全策略》服务》network conltrol service 调为手动，并取消相关权限
#### 软件限制
打算通过导出软件签名后    通过域服务器软件策略 禁止软件
