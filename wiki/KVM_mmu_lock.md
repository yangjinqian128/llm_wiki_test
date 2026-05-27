---
type: knowledge-node
tags: [core-concept, auto-generated, KVM, ARM64, locking]
last_compiled: 2026-05-22
---

# KVM mmu_lock

KVM ARM64 使用读写锁 (rwlock) 保护 stage-2 页表操作[^1]。

## 读锁场景 (user_mem_abort)

- 使用 `KVM_PGTABLE_WALK_SHARED` 标志[^1]
- 多 vCPU 可并发处理 fault[^1]
- 使用 cmpxchg 处理 PTE 并发修改[^1]
- 配合 mmu_invalidate_retry 检测页面失效[^1]

## 写锁场景

| 场景 | 触发源 | 为什么需要写锁 |
|------|--------|----------------|
| `kvm_phys_addr_ioremap` | 用户 ioctl | 独占操作，无 SHARED 标志 |
| `kvm_mmu_split_huge_pages` | Dirty logging | 修改页表结构 |
| `kvm_stage2_wp_range` | Dirty logging | 批量修改 PTE 权限 |
| `stage2_unmap_vm` | VM reset | 大规模清空映射 |
| `kvm_arch_flush_shadow_memslot` | Memslot 操作 | 清空指定范围 |
| `kvm_free_stage2_pgd` | VM 销毁 | 原子清除页表指针 |

## PTE 更新方式

写锁路径：直接 `WRITE_ONCE(*ctx->ptep, new)`[^1]

读锁路径：使用 `cmpxchg(ctx->ptep, ctx->old, new)`[^1]

## 相关概念

- [[KVM]]
- [[Stage2大页拆分]]
- [[脏页跟踪]]
- [[锁机制]]

[^1]: 来源: raw/notes/ai_gen/KVM_ARM64_mmu_lock_write_lock_scenarios.md