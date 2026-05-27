---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, migration]
last_compiled: 2026-05-22
---

# Multifd

Multifd 是 QEMU [[热迁移]] 的多通道并行传输框架[^1]。

## 核心作用

- 多通道并行传输 RAM 页面[^1]
- 每个通道独立压缩[^1]
- 提高迁移吞吐量[^1]

## 同步模式

| 模式 | 描述 |
|------|------|
| **per-section** (legacy) | 每个 RAM_SAVE_FLAG_EOS 后全通道 SYNC |
| **per-round** (modern) | 每完整扫描一轮后一次 SYNC |

## 数据流

```
源端 multifd_send_setup()
  → 创建 N 个发送线程
  → 每线程独立压缩通道

页面队列 → multifd_queue_page()
  → 分发到各通道

目的端 multifd_recv_setup()
  → 创建 N 个接收线程
  → 解压缩写入内存
```

## 相关概念

- [[热迁移]]
- [[脏页跟踪]]
- [[XBZRLE]]

[^1]: 来源: raw/notes/ai_gen/QEMU_Live_Migration_Flow.md