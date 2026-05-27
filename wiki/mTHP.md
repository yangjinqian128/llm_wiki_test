---
type: knowledge-node
tags: [core-concept, auto-generated, Linux, memory, hugepage]
last_compiled: 2026-05-22
---

# mTHP

mTHP (Multi-size Transparent Huge Pages) 是 Linux 内核支持多种粒度大页的特性[^1]。

## Contiguous PTE 折叠/展开

mTHP 在 ARM64 上利用 [[Contiguous PTE]] 硬件机制[^1]。

**折叠 (fold)**: 4 个非 contiguous PTE → 1 个 contiguous block[^1]

**展开 (unfold)**: 1 个 contiguous block → 4 个独立 PTE[^1]

## BBML2 优化

[[BBML2]] 下跳过中间 TLBI+DSB[^1]：

```
BBML0: 清零 → TLBI → 写入 CONT
BBML2: 清零 → 写入 CONT (跳过 TLBI)
```

可能出现新旧 TLB entry 同时存在，BBML2 保证不产生 conflict abort[^1]。

## 代码位置

`arch/arm64/mm/contpte.c`[^1]：

```c
if (!system_supports_bbml2_noabort())
    __flush_tlb_range(&vma, start_addr, addr, PAGE_SIZE, true, 3);
```

## 相关概念

- [[Contiguous PTE]]
- [[BBML2]]
- [[页表]]
- [[内存管理]]

[^1]: 来源: raw/notes/ai_gen/ARM_CPU_BBML2_Analysis.md