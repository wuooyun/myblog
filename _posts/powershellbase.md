---
title: 关于powershell的一点点基础记录
date: 2019-11-14 16:49:51
tags:
 - powershell
categories:
 - windows
---
###  scripting ToolMaking 
jsnover

#### 测试未知命令的运行结果
**-whatif**   告诉你执行的结果 但实际不会执行操作
**-confirm**  操作执行前需要确认

#### 输出到指定格式
 converto-html  
 converto-csv

onverto-CMD 输出到命令行中 可以加参数  可作为管道继续使用 

out-file 一般配合converTo 使用 输出到文件

export-csv  直接输出到文件


get-moudle  -list   获取模块信息

```powershell
$var=read-host #获取控制台输入
 write-host 输出到控制台 命令执行后管道中将获取不到对象 # write-host 会删除所有对象 并输出到控制台
 write-output 输出的控制台 且管道可操作
 write-warning 输出警告信息
 write-error  
param(
   $name=“小明”
) #获取用户输入 a.ps1 xiaofang

[CmdLetBinding()]
param(
   [Parameter(mandatory=$True)] # 配合[CmdLetBinding()] 表明下一个参数强制用户输入
  $name = “小明”
)
```
###  powershell ISE    
 新建空白ps1  Ctrl+j获取模板  

