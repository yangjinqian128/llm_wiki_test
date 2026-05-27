---
type: knowledge-node
tags: [core-concept, auto-generated, iommu-page-table]
last_compiled: 2026-05-22
---

# IO页表

[[IO页表]] 是 [[IOMMU]]/[[SMMU]] 进行地址翻译的核心数据结构[^1]。

## 页表层级

ARM64 [[SMMUv3]] 48bit、4K 页大小的页表划分[^1]：

| Level | Block Size |
|-------|------------|
| L0    | -          |
| L1    | 1G         |
| L2    | 2M         |
| L3    | 4K         |

每级页表有 512 个 entry，每个 entry 64bit[^1]。

## 初始化流程

`arm_smmu_attach_dev` 创建页表操作函数[^1]：

```
alloc_io_pgtable_ops
  -> arm_64_lpae_alloc_pgtable_s1
    -> arm_lpae_alloc_pgtable
      -> map = arm_lpae_map
      -> unmap = arm_lpae_unmap
      -> iova_to_phys
    -> __arm_lpae_alloc_pages (创建 pgd)
```
[^1]

## 映射流程

`iommu_dma_alloc_remap` 分配离散物理页面[^1]。

`__iommu_dma_alloc_pages` 按 block 大小分配[^1]。

`iommu_map_sg_atomic` 建立 iova 到物理页面的映射[^1]。

## 地址翻译

`__arm_lpae_map` 递归创建页表[^1]。

输入：iova、paddr、size、iommu_prot[^1]。

[^1]: 来源：raw/notes/iommu/io_pgtable_note