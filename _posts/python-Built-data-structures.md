---
title: 常用Python内建数据结构
date: 2019-12-25 15:27:38
tags:

categories:
 - python
---

1.  列表作为栈使用

列表方法使得列表作为堆栈非常容易，最后一个插入，最先取出("后进先出")。要添加一个元素到堆栈的顶端，使用** append（）** 。要从堆栈顶部取出一个元素，使用** pop() **，不用指定索引。例如
``` python
>>> stack = [3, 4, 5]
>>> stack.append(6)
>>> stack.append(7)
>>> stack
[3, 4, 5, 6, 7]
>>> stack.pop()
7
>>> stack
[3, 4, 5, 6]
>>> stack.pop()
6
>>> stack.pop()
5
>>> stack
[3, 4]

```
2.  列表作为队列使用
列表也可以用作队列，其中先添加的元素被最先取出("FIFO"),然鹅列表用作这个目的效率极低。因为列表在魔王添加和弹出元素非常快，但是在开头插入或弹出元素却很慢（因为所有后面的元素都必须移动一位）。
若要实现一个队列，可以使用 collections.deque,它被设计成可以快速地从两端添加或弹出元素例如
```python3
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])
```

