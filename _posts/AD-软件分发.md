---
title: 浅谈windows域下软件分发
date: 2019-01-22 16:40:48
tags:
 - AD域
 - 软件分发
categories:
 - windows
---

思路一：将。exe安装包重新打包为msi软件包 然后通过脚本自动安装

问题：有些exe是否支持打包为msi

思路二: 推送脚本到用户 开机自动执行安装
