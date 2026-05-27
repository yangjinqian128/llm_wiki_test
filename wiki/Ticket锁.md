---
title: Ticket锁
aliases: [ticket lock, ticket spinlock]
tags: [同步机制, 算法]
---

# Ticket锁

Ticket 锁是保证自旋锁公平性的算法，解决简单自旋锁的无序争抢问题。

## 数据结构

```c
struct __raw_tickets {
    u16 next;   // 下一个 ticket
    u16 owner;  // 当前持有者
};
```

## 加锁逻辑

1. 原子读取并增加 next
2. 保存返回值作为自己的 ticket
3. spin 等待 owner == ticket

```
CPU0      CPU1      CPU2      CPU3
next: 0   next: 1   next: 2   next: 3
          ticket=1  ticket=2  ticket=3
          spin      spin      spin

unlock: owner++
owner: 1 -> CPU1获得锁
```

## 解锁逻辑

增加 owner，下一个 ticket 的等待者获得锁。

## 解决的问题

保证公平性：先请求的先获得锁。

## 未解决的问题

仍有 cache 问题：
- 所有等待者读取同一个 owner
- owner 变化时所有 cache 被无效化

## 后续发展

[[MCS锁]] 和 [[qspinlock]] 解决 cache 问题。

## 内核状态

ARMv6 内核代码中仍有保留，现代内核使用 qspinlock。

[^1]: 来源: raw/notes/linux/Linux内核spinlock实现分析