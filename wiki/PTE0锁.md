---
type: knowledge-node
tags: [core-concept, auto-generated, KVM, ARM64, concurrency]
last_compiled: 2026-05-22
---

# PTE[0]锁

PTE[0]-as-Block-Lock 是 KVM Stage-2 在 [[Contiguous PTE]] 操作时的并发控制机制[^1]。

## 设计背景

[[Contiguous PTE]] 的 BBM 需要同时修改 N 个 PTE[^1]：

- PTE[0] 用作整个 contig block 的锁
- 其他 PTE[1..N-1] 不单独加锁

## 状态分类

| PTE[0] 状态 | 处理 |
|-------------|------|
| LOCKED | 返回 -EAGAIN，退让重试 |
| Valid + CONT | 返回 -EAGAIN，等待其他路径解除 |
| Valid + 无 CONT | 正常单页 BBM |

## 并发正确性

单页路径检查 PTE[0] 状态[^1]：

```
if (stage2_pte_is_locked(pte0))
    return -EAGAIN;  // 有线程操作该 contig block

if (pte0 & KVM_PTE_LEAF_ATTR_CONT)
    return -EAGAIN;  // 存在完整 contig block，不能单页修改
```

## 竞态解除

状态 2 (Valid + CONT) 依赖其他路径解除[^1]：

- MMU notifier 回调
- wrprotect unfold

## 相关概念

- [[Contiguous PTE]]
- [[BBML2]]
- [[KVM_mmu_lock]]
- [[锁机制]]

[^1]: 来源: raw/notes/ai_gen/PTE0_Block_Lock_分析.md