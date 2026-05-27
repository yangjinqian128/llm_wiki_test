---
title: ACQUIRE语义
aliases: [acquire semantics, ACQUIRE操作]
tags: [并发编程, 内存模型]
---

# ACQUIRE语义

ACQUIRE 是单向内存屏障，保证后续操作在 ACQUIRE 之后发生。

## 定义

ACQUIRE 后的内存操作不能重排到 ACQUIRE 之前，但之前的操作可以重排到之后。

## 包含操作

- LOCK 操作（锁获取）
- `smp_load_acquire()`
- `smp_cond_load_acquire()`

## 典型用法

```c
// 获取锁
spin_lock(&lock);  // ACQUIRE
// 临界区操作保证在锁获取后
data = *p;
```

## 与 RELEASE 配对

ACQUIRE 和 [[RELEASE语义]] 配对使用，保证临界区操作的可见性。

## 与通用屏障区别

ACQUIRE+RELEASE 不构成完整屏障，但保证：
- ACQUIRE 后可见 RELEASE 前的所有操作
- 临界区内操作按序完成

## 地址依赖

ACQUIRE 语义适用于读操作，原子操作的 ACQUIRE 仅对读部分生效。

## 相关概念

- [[RELEASE语义]]: 配对使用
- [[内存屏障]]: 完整排序
- [[RCU]]: 读侧类似 ACQUIRE

[^1]: 来源: raw/notes/linux/Linux内核memory-barrier文档学习