---
title: mutex
aliases: [互斥锁]
tags: [sync, lock, concurrent]
---

# mutex

互斥锁，睡眠等待的锁机制。

## 与spinlock对比

| 锁 | 等待方式 | 适用场景 |
|------|----------|----------|
| mutex | 睡眠 | 持锁时间长 |
| spinlock | 自旋 | 持锁时间短 |

参见[[spinlock]]、[[锁机制]]。

## 相关概念

- [[spinlock]] - 自旋锁对比
- [[锁机制]] - 锁分类
- [[并发编程]] - 同步机制