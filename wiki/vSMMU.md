---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization, smmu]
last_compiled: 2026-05-22
---

# vSMMU

[[vSMMU]]是虚拟机里的SMMU模拟[^1]。

## 实现方式

两种实现[^1]：
1. **纯软件模拟**：完全用软件实现[^1]
2. **硬件支持**：nested SMMU实现[^1]

## vSVA支持

[[vSMMU]]支持[[vSVA]]需要支持page fault[^1]。

## 地址翻译

- **S1**：VA → IPA（guest页表）[^1]
- **S2**：IPA → PA（vfio配置）[^1]

## 页表同步

guest页表变化需同步到物理[[SMMU]][^1]。

需要更新页表项和[[TLB]][^1]。

## CD配置

guest CD地址(IPA)配置给物理SMMU[^1]。

SMMU硬件对CD基地址做S2翻译[^1]。

## 缺页处理

S1缺页上报给guest[^1]。

qemu里[[SMMU驱动]]处理缺页[^1]。

## 运行时调用

1. STE/CD install[^1]
2. TLB/CONFIG invalidate[^1]

## 相关概念

- [[SMMU]] - 系统MMU
- [[vSVA]] - 虚拟化SVA
- [[iommufd]] - IOMMU接口框架
- [[PASID]] - 进程地址空间ID

[^1]: 来源：raw/notes/virt/vSVA_logic
[^2]: 来源：raw/notes/virt/基于iommufd的vSMMU基本逻辑分析