---
title: blk-mq
aliases: [Block Multi-Queue]
tags: [block, io, queue]
---

# blk-mq

多队列块层架构，提高高并发存储性能。

## 架构

```
软件队列(多个) → 硬件队列 → 设备驱动
```

参见[[块设备子系统]]。

## 相关概念

- [[块设备子系统]] - 块层架构
- [[BIO]] - 基本I/O结构
- [[sbitmap]] - 可扩展位图分配