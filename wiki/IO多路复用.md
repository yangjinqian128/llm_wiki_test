---
title: IO多路复用
aliases: [I/O Multiplexing]
tags: [io, syscall, concurrent]
---

# IO多路复用

同时监控多个文件描述符的机制。

## 系统调用

| 调用 | 特点 |
|------|------|
| poll | 逐次传入fd数组 |
| select | 固定fd数量限制 |
| epoll | 事件驱动，高性能 |

参见[[poll系统调用]]。

## 驱动实现

参见[[poll系统调用]]中的驱动poll回调实现。

## 相关概念

- [[poll系统调用]] - poll实现
- [[等待队列]] - poll等待机制
- [[UACCE]] - 加速器poll实现