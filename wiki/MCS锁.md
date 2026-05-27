---
title: MCS锁
aliases: [MCS lock]
tags: [同步机制, 算法]
---

# MCS锁

MCS 锁是解决自旋锁 cache 问题的算法，由 Mellor-Crummey 和 Scott 提出。

## 核心思想

每个等待者有自己的锁副本节点，在自己的节点上 spin，而不是全局共享位置。

## 数据结构

```c
struct mcs_spinlock {
    int locked;         // 自己的锁标记
    struct mcs_spinlock *next;  // 下一个等待者
};
```

## 加锁流程

1. 找到锁链表尾部
2. 将自己的 node 挂到尾部
3. 在自己 node 的 `locked` 上 spin

```
      tail
        |
        v
  node0 -> node1 -> node2 (spin on locked)
```

## 解锁流程

1. 将下一个 node 的 `locked` 设为 1
2. 下一个等待者获得锁

## 解决的问题

1. **公平性**: FIFO 顺序获得锁
2. **cache 问题**: 每个 CPU spin 在本地变量

## 内存占用

每个锁需要一个 tail 指针 + 每个 CPU 一个 node。

## Linux 内核

内核定义了 MCS node 结构，但实际使用 [[qspinlock]] 变种，优化内存占用。

## 相关概念

- [[qspinlock]]: MCS 的优化实现
- [[内存屏障]]: node 间传递需要 barrier

[^1]: 来源: raw/notes/linux/Linux内核spinlock实现分析