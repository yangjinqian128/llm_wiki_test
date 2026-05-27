---
title: blkring
aliases: [Block Ring, 无锁环形队列]
tags: [concurrent, lockfree, queue]
source: raw/notes/blkring基本逻辑分析.md
---

# blkring

ARM开发的并发计算库（progress64）中的无锁队列实现，具有高并发吞吐量。源码位于`src/p64_blkring.c`。

## 内存布局

```
+---------------------------------------------------------+
|                     p64_blkring_t                       |
+----------------------------+----------------------------+
|  cons (cacheline0)         |  prod (cacheline1)         |
|  +----------+----------+   |  +----------+----------+   |
|  |  head    |  mask    |   |  |  tail    |  mask    |   |
|  +----------+----------+   |  +----------+----------+   |
+----------------------------+----------------------------+
|                        ring[]                           |
|  slot: {sn(8B), elem(8B)}                               |
+---------------------------------------------------------+
```

- cons/prod各独占一个缓存行，避免false sharing
- 生产者只写prod.tail，消费者只写cons.head

## 入队逻辑

1. `atomic_fetch_add`抢占序号区间
2. 使用swizzle函数错开cache line访问
3. ARM LSE分支使用CASP原子写入
4. 非LSE分支自旋等待

## 出队逻辑

1. `atomic_fetch_add`抢占序号区间
2. 等待slot有数据
3. 清空elem并更新sn为`sn + mask + 1`

## 相关概念

- [[无锁队列]] - 无锁并发数据结构
- [[原子操作]] - CAS/Fetch-Add等
- [[内存屏障]] - 确保操作顺序可见
- [[块设备子系统]] - 块层应用场景