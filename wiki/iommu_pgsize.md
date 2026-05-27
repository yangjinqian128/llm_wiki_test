---
type: knowledge-node
tags: [core-concept, auto-generated, IOMMU, DMA]
last_compiled: 2026-05-22
---

# iommu_pgsize

iommu_pgsize 决定 IOMMU 映射的最优粒度，影响是否使用 block mapping[^1]。

## 选择原则

`iommu_pgsize()` 函数选择逻辑[^1]：

1. `pgsize_bitmap & size` - 不超过剩余长度
2. `(iova | paddr)` 的 LSB 对齐约束筛选
3. `(iova ^ paddr)` 对齐检查 - IOVA 和 PA 必须同级对齐
4. 返回最大的可行 pgsize

## 示例

ARM64 4KB granule、3-level paging[^1]：

| 条件 | 结果 |
|------|------|
| iova=0x10000000, paddr=0x80000000, size=2MB | **映射为 2MB block** |
| iova=0x10004000 | **映射为 4KB page** |
| size=1GB, iova/PA 都 1GB 对齐 | **映射为 1GB block** |

## VFIO 控制参数

```c
static bool disable_hugepages;
module_param_named(disable_hugepages, disable_hugepages, bool, ...);
```

`disable_hugepages=1` 时强制每次映射 4KB[^1]。

## 相关概念

- [[SMMU]]
- [[VFIO]]
- [[IO页表]]

[^1]: 来源: raw/notes/ai_gen/VFIO_SMMU_Hugepage_Mapping_and_Split.md