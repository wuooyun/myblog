---
title: Ubuntu Server18.04安装samba
date: 2018-09-06 13:39:39
tags:
- samba
- linux
categories:
- samba
---
# Ubuntu Server18.04安装samba实现windows共享文件夹

[参考文档](https://blog.csdn.net/yuanjinshenglife/article/details/81122132)

## 一、更换服务器更新源为国内源(以中科大源为例)
可自行[百度](baidu.com)
```bash
wget https://mirrors.ustc.edu.cn/repogen/conf/ubuntu-https-4-bionic -O sources.list
sudo mv /etc/apt/sources.list /etc/apt/sources.list.bak
sudo mv sources.list /etc/apt/sources.list
sudo apt update
sudo apt upgrade
```
***

## 二、利用apt工具安装samba

```shell
sudo apt-get install samba
```
### 1. 在/home下创建共享文件夹，名为myshare
```shell
mkdir /home/myshare
```
### 2. 修改myshare目录的权限
```shell
chmod 777 /home/myshare
```
### 3. 修改samba配置文件
```shell
vim /etc/samba/smb.conf
```
![smb.conf](http://wx4.sinaimg.cn/mw690/0061nvdWgy1fuzu4a1cdlj30ht0elt9g.jpg)
### 4. 创建samba账号
4.1在 /etc/samba/下创建一个名为smbpasswd的文件
touch /etc/samba/smbpasswd
4.2创建一个名为samba(用户名自己定义)的samba账号
```shell
useradd samba
smbpasswd - a samba
```
上面命令执行后，输入两次密码samba账户创建sssssambamba服务

### 5. 对配置进行了更改后，需要重启samba服务后更改的配置才会生效

/etc/init.d/smbd restart   或 service smbd restart

### 6. 在window系统中输入访问地址访问即可

windows徽标+R 在弹出的运行窗口中输入 \\ip即可访问。如\\192.168.1.121,输入samba用户名及密码访问即可看到共享，然后就可以在Linux系统与Windows系统直接进行文件共享了



 
