---
title: Cache一致性
aliases: [MESI协议, Cache Coherence]
tags: [hardware, cache, coherence]
---

# Cache一致性

多核系统中保持各核Cache数据一致的机制。

## MESI协议

| 状态 | 说明 |
|------|------|
| M | Modified，独占修改 |
| E | Exclusive，独占干净 |
| S | Shared，共享干净 |
| I | Invalid，无效 |

## Store Buffer与Invalid Queue

参见[[Load-Store单元]]，影响多核可见性。

## 相关概念

- [[Cache]] - 缓存结构
- [[内存屏障]] - 保证顺序可见
- [[Load-Store单元]] - cache交互
- [[并发编程]] - 一致性编程