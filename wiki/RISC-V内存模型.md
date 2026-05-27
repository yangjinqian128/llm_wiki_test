---
title: RISC-V内存模型
created: 2026-05-22
tags:
  - RISC-V
  - 内存序
  - 并发
---

# RISC-V内存模型

## 概述

RISC-V采用弱内存序(WMO, Weak Memory Ordering)，通过规则约束内存访问顺序，其余情况允许乱序执行。

## 内存序 vs Cache一致性

- **内存序(Consistency)**：硬件故意放松指令执行顺序以提升性能
- **Cache一致性(Coherence)**：多核共享内存的数据一致性，通过MESI等协议解决

## 内存序规则

### Overlapping-Address Orderings

| 规则 | 描述 |
|------|------|
| 1 | store与重叠地址操作保序 |
| 2 | load-load重叠地址，需考虑写入来源 |
| 3 | AMO/SC后的load保序 |

注意：普通store-load可以乱序（TSO唯一放松点）。

### Explicit Synchronization

| 规则 | 描述 |
|------|------|
| 4 | FENCE指令保序 |
| 5 | acquire语义保序 |
| 6 | release语义保序 |
| 7 | RCsc标注保序 |
| 8 | lr/sc pair保序 |

### Acquire/Release语义

**Acquire**：后面的读写不能排到acquire之前

```
         +-------------+
         | load/store  |---------+
         +-------------+         |
                                 |
----- instruction with acquire --+--
```

**Release**：前面的读写不能排到release之后

### Syntactic Dependencies

| 规则 | 描述 |
|------|------|
| 9 | 地址依赖保序 |
| 10 | 数据依赖保序 |
| 11 | 控制依赖保序（仅store） |

### Pipeline Dependencies

| 规则 | 描述 |
|------|------|
| 12 | 依赖链条保序 |
| 13 | 依赖链条保序 |

## RCsc vs RCpc

- **RCsc**：acquire/release间不能乱序
- **RCpc**：acquire/release间可乱序

RISC-V WMO使用RCsc。

## Barrier指令

- **fence**：内存屏障
- **fence.i**：指令缓存屏障

参见[[ARM内存模型]]、[[内存可见性]]。

---

**参考来源**：[riscv_memory_order](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/riscv/riscv_memory_order)