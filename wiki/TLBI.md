---
type: knowledge-node
tags: [core-concept, auto-generated, iommu, tlb]
last_compiled: 2026-05-22
---

# TLBI

[[TLBI]]（TLB Invalidate）无效化[[SMMU]]的TLB条目[^1]。

## VHE支持

Host kernel在EL2运行时，DVM TLBI广播需在相同EL生效[^1]。

配置方式：
- 设置STE的STRW[^1]
- 设置SMMU CR2.E2H使ASID标记生效[^1]
- TLBI命令从TLBI_NH改为TLBI_EL2[^1]

## TLBI命令类型

| 命令 | 作用范围 |
|------|----------|
| TLBI_VA | 特定地址[^1] |
| TLBI_ASID | 特定ASID的所有TLB[^1] |
| TLBI_EL2_VA | EL2地址[^1] |
| TLBI_EL2_ASID | EL2 ASID[^1] |

## Range Invalidate

当SMMU支持`IDR3_RIL=1`时，可使用带VA的TLBI按范围无效化[^1]。

命令字段：scale, num, asid, address, tg, ttl, leaf[^1]。

## Linux实现

### Strict模式

```c
iommu_dma_free
  → __iommu_dma_unmap
      → iommu_iotlb_gather_init
      → iommu_unmap_fast
          → arm_smmu_unmap
              → arm_lpae_unmap
                  → io_pgtable_tlb_add_page
                      → arm_smmu_tlb_inv_page_nosync
                          → iommu_iotlb_gather_add_page
      → iommu_tlb_sync
          → arm_smmu_iotlb_sync
              → arm_smmu_tlb_inv_range
```
[^1]

### Non-strict模式

使用定时器，队列满或定时器到期时执行TLBI[^1]：

```c
iova_domain_flush
  → iommu_dma_flush_iotlb_all
      → arm_smmu_flush_iotlb_all
          → arm_smmu_tlb_inv_context  // 带ASID的TLB
```
[^1]

## 相关概念

- [[SMMU]] - TLB硬件
- [[BBML]] - 页表同步机制
- [[IOPF]] - 缺页处理
- [[SVA]] - 共享虚拟地址

[^1]: 来源：raw/notes/iommu/smmu_tlbi_note