---
type: knowledge-node
tags: [core-concept, auto-generated, SMMU, VFIO]
last_compiled: 2026-05-22
---

# SMMU脏页跟踪

SMMU 侧脏页跟踪支持设备 DMA 修改的内存追踪[^1]。

## 当前支持状态

| 路径 | 是否支持 | 机制 |
|------|---------|------|
| VFIO type1 | 支持（软件） | bitmap 在 vfio_dma 层 |
| iommufd S1 | 支持（硬件） | [[HTTU]] Hardware Dirty |
| iommufd S2 | 不支持 | -EOPNOTSUPP |

## VFIO type1 软件路径

```
ioctl(VFIO_IOMMU_DIRTY_PAGES, START)
  → 为每个 vfio_dma 分配软件 bitmap

ioctl(VFIO_IOMMU_DIRTY_PAGES, GET_BITMAP)
  → 遍历 dma->bitmap，copy_to_user
```

不涉及 SMMU 页表拆分[^1]。

## iommufd S1 HTTU 路径

[[HTTU]] 硬件自动标记脏页[^1]：

- DBM=1 的 writable PTE
- 硬件写操作自动清除 AP_RDONLY
- 软件遍历页表检测脏状态

不需要拆分 block mapping[^1]。

## S2 未来支持

若实现无 HTTU 的 S2 dirty tracking，需要[^1]：

- 拆分 block → 4KB PTEs
- [[BBML2]] 成为硬性前提

## 相关概念

- [[HTTU]]
- [[VFIO]]
- [[BBML2]]
- [[脏页跟踪]]

[^1]: 来源: raw/notes/ai_gen/VFIO_SMMU_Hugepage_Mapping_and_Split.md