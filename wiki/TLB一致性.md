---
type: knowledge-node
tags: [core-concept, auto-generated, ARM64, memory, TLB]
last_compiled: 2026-05-22
---

# TLB一致性

TLB 一致性是多核系统中保证各 CPU TLB 内容同步的机制[^1]。

## 硬件机制

ARM64 TLB 失效指令[^1]：

- `TLBI VAE1IS` - 虚地址失效（Inner Shareable）
- `TLBI VMALLE1IS` - 全 VM 失效
- `TLBI PAALL` - 物理地址失效

## 软件同步

[[BBML2]] 修改页表后需要 TLB 失效[^1]：

```
修改 PTE → DSB → TLBI → DSB → ISB
```

## KVM Stage-2

TLB 失效时机[^1]：

- `stage2_try_break_pte()` 后[^1]
- 页表结构变更后[^1]
- VM switch 时[^1]

## 批量失效

`kvm_tlb_flush_vmid_range()` 批量 TLBI[^1]：

- 使用 TLBI range 指令
- 减少单独 TLBI 次数

## 相关概念

- [[TLB]]
- [[BBML2]]
- [[KVM]]
- [[页表]]

[^1]: 来源: raw/notes/ai_gen/ARM64_KVM_Stage2_Contig_Hugetlb_设计文档.md