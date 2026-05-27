---
title: qspinlock
aliases: [queued spinlock, MCS spinlock]
tags: [同步机制, Linux内核]
---

# qspinlock

qspinlock 是现代 Linux 内核采用的队列式自旋锁实现，解决了传统锁的公平性和缓存问题。

## 发展历程

1. 简单自旋锁: 无序争抢，cache 抢夺严重
2. Ticket 锁: 保证公平，仍有 cache 问题
3. MCS 锁: 解决 cache 问题，占用内存多
4. qspinlock: MCS 变种，内存占用小

## 数据结构

```c
struct qspinlock {
    u8  locked;      // 锁状态
    u8  pending;     // 等待位
    u16 tail;        // 排队节点编码
};
```

## MCS Node

每个 CPU 预分配 4 个 MCS node:
- task context
- hardirq context
- softirq context  
- NMI context

## 加锁逻辑

1. 尝试 `locked = 0 -> 1`: 直接获得锁
2. `pending = 1`: 在 locked 上 spin
3.排队: 在 MCS node 上 spin，等待通知

## 排队示意

```
lock: [tail=cpu3] [pending=1] [locked=1]
        |
        v
      mcs_node0(cpu0) -> mcs_node1(cpu3)
```

## 解锁逻辑

- 无排队: `locked = 0`
- 有 pending: `pending -> locked`
- 有排队: 唤醒下一个 MCS node

## 与 spinlock_t 关系

```c
spinlock_t -> raw_spinlock_t -> qspinlock
```

## ARM64 支持

ARM64 使用 qspinlock 实现 `spinlock_t`。

## 相关概念

- [[MCS锁]]: qspinlock 的理论基础
- [[内存屏障]]: lock/unlock 使用 barrier
- [[内核抢占]]: spinlock 关抢占

[^1]: 来源: raw/notes/linux/Linux内核spinlock实现分析