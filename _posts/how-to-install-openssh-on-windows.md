---
title: 如何在Windows 7/10上安装OpenSSH
date: 2019-04-12 10:50:48
tags:
 - openssh
 - windows
Categories: 
 - windows
---

### 如何在Windows 7/10上安装OpenSSH

在Windows上使用SSH，如何在Windows操作系统上的同一用户会话登录中运行/启动远程计算机上的图形程序。

## 要求
- OpenSSH (可以从github上官方存储库下载二进制文件https://github.com/PowerShell/Win32-OpenSSH/releases )
- Pstoos (Microsoft的官方有用工具https://docs.microsoft.com/en-us/sysinternals/downloads/pstools )
- PowerShell

### 安装Pstools (由Microsoft提供）
1. 下载工具
2. 复制“C:\Windows\System32\”下的文件夹PSTools的内容。
3. 以管理员身份打开cmd并运行C\Windows\System32\psexec.exe，接受eula许可证。

### 在Windows 7/10上安装SSH服务器
1. 下载最新的OpenSSH for Windows二进制文件（包OpenSSH-Win64.zip或OpenSSH-Win32.zip）
2. 作为管理员，将包解压缩到％PROGRAMFILES％\OpenSSH 
   注意：该文件夹必须命名为“OpenSSH”
3. 以管理员身份打开PowerShell（右键单击PowerShell图标，“以管理员身份运行”），将目录更改为“C:\Program Files\OpenSSH”使用该命令安装sshd和ssh-agent服务

``` powershell
> cd“％PROGRAMFILES％\OpenSSH”
> powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps
```
4. 允许在Windows防火墙中与SSH服务器建立传入连接：
- 以管理员身份运行以下PowerShell命令（Windows 8和2012或更新版本）：
```powershell
New-NetFirewallRule -Name sshd -DisplayName'OpenSSH Server（sshd）'-Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22

```
- 或转到“控制面板”>“系统和安全”>“Windows防火墙”>“高级设置”>“入站规则”，然后为端口22添加新规则。
5. 启动服务和/或配置自动启动：
◦转到“控制面板”>“系统和安全”>“管理工具”，然后打开“服务”。找到sshd服务。
◦如果希望服务器在计算机启动时自动启动：转到“操作”>“属性”。在“属性”对话框中，将“启动类型”更改为“自动”并确认。
◦单击“启动服务”以启动sshd服务。
6. 在C:\Users\<用户>\.ssh下创建~./ssh文件夹
7. 在~./.ssh下创建文件“authorized_keys”
8. 运行scrips以修复/检查具有管理员权限的PowerShell的正确权限。
```powershell
> powershell.exe -ExecutionPolicy Bypass -File FixHostFilePermissions.ps1 
> powershell.exe -ExecutionPolicy Bypass -File FixUserFilePermissions.ps1
```
9. 个性化您的SSH服务器设置，编辑配置文件％PROGRAMDATA％\ssh\sshd_config。

### 在Windows 7/10上安装SSH客户端

1. 执行上一段“在Windows 7/10上安装SSH服务器”中的步骤1到2

### 在客户端上不使用密码启用公钥
1. 以管理员身份打开cmd.exe并运行ssh-keygen.exe并按Enter键以显示默认配置的所有消息
```powershell
> cd“％PROGRAMFILES％\ OpenSSH” 
> ./ssh-keygen.exe
```
### 在服务器上不使用密码启用公钥
1. 将私钥和公钥复制到要在服务器上使用的用户的〜.ssh文件夹中。运行ssh-add.exe将私钥和公钥添加到ssh-agent。
注意：确保ssh-agent正在运行。
```powershell
> ./ssh-add.exe
```

### 使用psexec.exe在Windows上运行远程计算机上的图形程序
使用ssh连接到远程计算机，并在打开的同一用户会话中运行远程计算机上的notepad.exe
```powershell
> cd“％PROGRAMFILES％\ OpenSSH” 
> ssh.exe [remote_local_user] @ [remote_ip] -i“C：\ Users \\。ssh \ id_rsa” 
> user @ remote_ip> psexec.exe \\ 127.0.0.1 -d -i -s notepad.exe
```


