---
title: linux文件夹文件按照时间排序
date: 2019-01-03 15:16:35
tags:
 - ls
 - sort
 - Linux
categories:
 - Linux
---

### 1，按照时间升序
```bash
命令:ls -lrt
详细解释:

-l     use a long listing format  以长列表方式显示（详细信息方式）
-t     sort by modification time 按修改时间排序（最新的在最前面）
-r     reverse order while sorting （反序）
```
### 2，按照时间降序（最新修改的排在前面）
```bash
命令:ls -lt
详细解释:

-l     use a long listing format  以长列表方式显示（详细信息方式）
-t     sort by modification time 按修改时间排序（最新的在最前面）
```
