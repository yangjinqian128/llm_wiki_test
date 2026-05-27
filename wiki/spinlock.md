---
type: knowledge-node
tags: [core-concept, auto-generated, synchronization]
last_compiled: 2026-05-22
---

# spinlock

[[spinlock]] 是 Linux 内核中用于短临界区的自旋锁实现[^1]。

## 基本行为

各 CPU core 互斥处理数据，加锁后只有持有者可操作[^1]。

整个过程关闭调度[^1]。

## 实现演进

### 原始实现

用原子 CAS 指令抢标记位[^1]。

问题：
1. 请求顺序与获得顺序不一致[^1]
2. 多核 cache 相互影响[^1]

### ticket spinlock

使用 `next` 和 `owner` 两个计数器[^1]。

获取锁时原子增加 `next`，等待 `owner` 与自己 ticket 相等[^1]。

解决问题1，未解决问题2[^1]。

### MCS spinlock

每个 CPU 有锁副本，用链表链接[^1]。

加锁时 spin 在自己的副本上[^1]。

解锁时沿链表依次传递[^1]。

解决两个问题但内存占用大[^1]。

### qspinlock

当前内核主流实现[^1]。

锁结构包含 `locked`、`pending`、`tail` 域段[^1]。

每个 core 有 4 个 qnode（对应 task、hardirq、softirq、NMI）[^1]。

核心逻辑：
- 第一个 core：直接 set `locked=1`[^1]
- 第二个 core：set `pending=1`，在 `locked` 上 spin[^1]
- 更多 core：在 MCS node 链表上排队[^1]

[^1]: 来源：raw/notes/linux/Linux内核spinlock实现分析