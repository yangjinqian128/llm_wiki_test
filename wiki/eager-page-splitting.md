---
type: knowledge-node
tags: [core-concept, auto-generated, KVM, virtualization]
last_compiled: 2026-05-22
---

# eager-page-splitting

Eager page splitting 是 KVM 主线版本改进，在迁移前预先拆分大页[^1]。

## v6.3 vs 主线

| 特性 | v6.3（旧版） | 主线版本 |
|------|------------|---------|
| **拆分时机** | Page fault 时按需 | 可提前或按需 |
| **Split 函数** | 无 | `kvm_pgtable_stage2_split()` |
| **迁移效率** | 可能传输多余数据 | 精确追踪 dirty page |

## v6.3 问题

1. **迁移效率低**：首次读取 dirty log 时传输整个 block[^1]
2. **Fault 风暴**：大量 Permission Fault[^1]
3. **拆分延迟**：只有实际访问时才拆分[^1]

## 主线改进

```
kvm_mmu_split_huge_pages(kvm, start, end)
  │
  ├─ kvm_pgtable_stage2_split()
  │    └─ stage2_split_walker()
  │         └─ 预拆分所有大页为 4K pages
  │
  └─ Stage-2 直接是 4K page mapping
       └
       └─ 迁移时 dirty bitmap 精确到 4K page
```

## 引入历史

- 2023年4月 Ricardo Koller 引入[^1]
- Commit: `8f5a3eb7513f` ("KVM: arm64: Add kvm_pgtable_stage2_split()")[^1]
- v6.4+ 合入主线

## 相关概念

- [[Stage2大页拆分]]
- [[脏页跟踪]]
- [[热迁移]]

[^1]: 来源: raw/notes/ai_gen/v6.3_VM_Migration_LargePage_Split.md