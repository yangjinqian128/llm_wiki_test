---
title: RCU
aliases: [Read-Copy-Update]
tags: [kernel, sync, concurrent]
---

# RCU

读-复制-更新同步机制，读操作无锁，写操作延迟更新。

## 特性

- 读操作零开销
- 写操作复制后延迟替换
- 等待所有读者完成后释放旧数据

## 应用

参见[[文件描述符引用计数]]，RCU用于查找file结构。

## 相关概念

- [[并发编程]] - RCU应用场景
- [[内存可见性]] - RCU可见性保证
- [[引用计数]] - RCU配合引用计数