---
type: knowledge-node
tags: [core-concept, auto-generated, KVM, ARM64, virtualization]
last_compiled: 2026-05-22
---

# Stage2大页拆分

Stage2大页拆分是 [[KVM]] 在 ARM64 上将 PMD block mapping 拆分为 4K PTE 映射的操作[^1]。

## 主要触发场景

使用 hugetlbfs backing 时，主要拆分场景[^1]：

| 类别 | 场景 | 是否真正拆小 |
|------|------|:---:|
| **脏页跟踪** | 主动拆页、增量拆页、写缺页 | **是** |
| **硬件故障** | MCE / memory failure | 可能 |
| **内存热下线** | memory offline | 临时 unmap |
| **嵌套虚拟化** | L2 stage-2 粒度限制 | **是** |
| **pKVM** | 权限/所有权精细控制 | **是** |
| **gmem** | guest_memfd 固定 4K | **是** |

## 脏页跟踪触发

最常见场景，热迁移必经之路[^1]：

```
开启脏页跟踪
  → kvm_mmu_split_huge_pages()
    → kvm_pgtable_stage2_split()
      → stage2_split_walker()
        → kvm_pgtable_stage2_create_unlinked()
          → 创建 4K 子页表
        → stage2_try_break_pte()
          → BBM 序列
        → stage2_make_pte()
          → 链接新页表
```

## 核心底层函数

- **`stage2_try_break_pte()`**: [[BBML2]] break-before-make 原语[^1]
- **`stage2_map_walk_leaf()`**: 映射粒度小于已有 block 时分配子页表[^1]
- **`kvm_pgtable_stage2_create_unlinked()`**: 创建离线子页表树[^1]

## 相关概念

- [[KVM]]
- [[SMMU]]
- [[VFIO]]
- [[脏页跟踪]]
- [[热迁移]]

[^1]: 来源: raw/notes/ai_gen/ARM64_KVM_Stage2_2M大页拆分场景.md