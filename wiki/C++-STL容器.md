---
title: C++ STL容器
tags: [cpp, stl, container]
created: 2026-05-22
source: raw/notes/C_C++/C++语法速记
---

# C++ STL容器

C++ STL常用容器[^1]。

## vector

动态数组，创建vector v时v.size()是0[^1]。

## queue

FIFO队列，只提供头尾操作[^1]。

## list

双向链表：

```
front                          back
+---+     +---+     +---+     +---+
| A |---->| B |---->| C |---->| D |
+---+     +---+     +---+     +---+
```

- `pop_front`: 移除A[^1]
- `pop_back`: 移除D[^1]
- `push_back`: D后添加[^1]
- `push_front`: 头部插入[^1]

### 迭代器失效

容器元素改变时，迭代器可能失效[^1]。

反向迭代器base方法得到后一个元素的基础迭代器[^1]。

## map

键值映射[^1]。

## 相关概念

- [[C++智能指针]]
- [[C++增强for循环]]
- [[C语言宏]]

[^1]: raw/notes/C_C++/C++语法速记