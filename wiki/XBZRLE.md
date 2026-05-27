---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, migration, compression]
last_compiled: 2026-05-22
---

# XBZRLE

XBZRLE 是 QEMU [[热迁移]] 的增量压缩技术[^1]。

## 工作原理

对脏页做 XOR 后 RLE (Run-Length Encoding)[^1]：

```
新页面 ← XOR ← 旧页面缓存
  → 结果做 RLE 压缩
  → 传输压缩数据
```

## 减少传输量

- 避免重复传输相同内容[^1]
- 只传输差异部分[^1]

## 启用条件

- 需要分配 XBZRLE 缓存[^1]
- 在 `ram_save_setup()` 中初始化[^1]

## 使用场景

- 脏页迭代传输[^1]
- 内存变化频繁但内容相似

## 相关概念

- [[热迁移]]
- [[脏页跟踪]]
- [[Multifd]]

[^1]: 来源: raw/notes/ai_gen/QEMU_Live_Migration_Flow.md