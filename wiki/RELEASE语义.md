---
title: RELEASE语义
aliases: [release semantics, RELEASE操作]
tags: [并发编程, 内存模型]
---

# RELEASE语义

RELEASE 是单向内存屏障，保证之前操作在 RELEASE 之前完成。

## 定义

RELEASE 前的内存操作不能重排到 RELEASE 之后，但之后的操作可以重排到之前。

## 包含操作

- UNLOCK 操作（锁释放）
- `smp_store_release()`

## 典型用法

```c
// 临界区操作
*p = data;
// 释放锁
spin_unlock(&lock);  // RELEASE
// 保证临界区操作在锁释放前完成
```

## 与 ACQUIRE 配对

[[ACQUIRE语义]] 和 RELEASE 配对使用，形成临界区保护：

```c
// CPU 1
*p = data;
smp_store_release(&ready, 1);  // RELEASE

// CPU 2
if (smp_load_acquire(&ready))  // ACQUIRE
    read_data(*p);  // 保证看到 CPU1 的写入
```

## Message Passing 模式

RELEASE+ACQUIRE 解决经典的 message passing 问题：
- 写数据 + 写标记(RELEASE)
- 读标记(ACQUIRE) + 读数据

## 与通用屏障区别

RELEASE 不保证后续操作排序，如需完全排序使用 `smp_mb()`。

## 控制依赖替代

两分支写相同值时，使用 RELEASE 替代控制依赖。

## 相关概念

- [[ACQUIRE语义]]: 配对使用
- [[内存屏障]]: 完整排序
- [[控制依赖]]: 某些场景可替代

[^1]: 来源: raw/notes/linux/Linux内核memory-barrier文档学习