---
title: powershell发现命令
date: 2019-04-15 10:27:39
tags:
 - powershell
 - 查找命令
---
### powershell 发现命令

 完成任何任务的第一步都是找出哪一个命令可以完成任务。我们首先找出如何完成打印机相关工作。这样，我们就可以通过运行命令，确保找到的命令能完成我们希望做的工作。然后，我们将该命令放入计划任务，一次只解决一个问题。
 我们首先开始寻找打印机相关的命令。注意我们选择*print*而不是*printer*作为关键字。如果可能，尽量使用单词更简短的形式，从而获得更多结果。
```powershell
help *print*
```
结果包含 name category Module

##  发现计划任务相关
```powershell
help *task*
```

可以看到结果的module中 ScheduledTasks module发现了很多相关命令
接下来我们查找该module有关的Command
```powershell
get-command -module scheduledtasks

```
接下来就简单了，我们需要哪个命令，可以用* help commond -full * 来查看相关命令的用法。
