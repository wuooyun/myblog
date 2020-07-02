---
title: 使用Gitea备份网络设备配置文件
date: 2019-12-03 09:40:38
tags:
 - git
 - gitea
 - 版本控制
categories:
 - Linux
 - git
---

### 背景

考虑到公司网络设备以及其他设备的配置文件的更新备份，及公司内部的运维脚本的保密性，现准备私有化部署类github的版本控制器。之前搭建了gitlab，gitlab虽然功能强大,但是占用硬件资源较高。结合我并没有特别复杂的需求，决定部署更轻量的gitea,以实现配置文件及脚本变更的记录。

### 准备

 当前环境
 位于2008r2中的vmware15虚拟机，安装Ubuntu18.04系统

### 开始安装

参考官方文档 http://docs.gitea.io

安装完成后 直接用用户名和密码即可通过http的方式push和clone


