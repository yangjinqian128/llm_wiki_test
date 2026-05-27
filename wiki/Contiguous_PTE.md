---
type: knowledge-node
tags: [core-concept, auto-generated, ARM64, TLB, page-table]
last_compiled: 2026-05-22
---

# Contiguous PTE

Contiguous PTE 是 ARM64 硬件机制，通过设置 PTE 的 BIT(52) (CONT bit) 将连续 N 个 PTE 合并为单个 TLB entry[^1]。

## 硬件要求

- 所有 PTE 指向物理地址连续且自然对齐的页面[^1]
- 所有 PTE 的属性（权限、内存类型等）完全一致[^1]
- 每个 PTE 的 CONT bit 置为 1[^1]

## 连续 PTE 数量

| 基础页 | CONT PTE | CONT PMD |
|--------|----------|----------|
| 4K | 64K (16 pages) | 32M |
| 16K | 2M (128 pages) | 1G |
| 64K | 2M (32 pages) | 16G |

## KVM Stage-2 支持

在 map、unmap、属性更新三个路径支持[^1]：

| 入口函数 | 操作 |
|----------|------|
| `kvm_pgtable_stage2_map()` | 支持设置 CONT bit |
| `kvm_pgtable_stage2_unmap()` | 识别并清除 contig block |
| `kvm_pgtable_stage2_wrprotect()` | unfold 后重写 |

## BBM 约束

修改 contig 区域内任意一个 PTE，若 TLB 缓存了该区域，视为不可预测行为[^1]。

必须走完整 [[BBML2]] 流程[^1]。

## 相关概念

- [[BBML2]]
- [[Stage2大页拆分]]
- [[TLB]]
- [[页表]]

[^1]: 来源: raw/notes/ai_gen/ARM64_KVM_Stage2_Contig_Hugetlb_设计文档.md