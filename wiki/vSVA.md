---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization-iommu]
last_compiled: 2026-05-22
---

# vSVA

[[vSVA]]（Virtual Shared Virtual Addressing）使虚拟机中的 IO 设备可以直接使用进程 VA[^1]。

## 基础要求

物理 IO 设备通过 [[VFIO]] 直通给虚拟机[^1]。

需要同时使能 [[SMMU]] 的 S1（VA→IPA）和 S2（IPA→PA）翻译[^1]。

## vSMMU

虚拟机中的 [[SMMU]] 称为 [[vSMMU]][^1]。

可以是纯软件模拟，也可以有硬件支持[^1]。

[[vSVA]] 需要支持 page fault 功能[^1]。

## Nested 翻译

设备 DMA 访问时，S1 翻译在物理硬件上发生[^1]。

虚拟机进程页表需被 host [[SMMU]] 感知[^1]。

Host [[SMMU]] 检测到 S1 缺页时，上报给 guest[^1]。

## 页表同步

虚拟机页表变化需同步到 host [[SMMU]][^1]。

包括页表项和 [[TLB]] 无效化[^1]。

使用 `VFIO_IOMMU_CACHE_INVALIDATE` ioctl[^1]。

## PASID 支持

Guest 多进程使用一个设备需支持 [[PASID]][^1]。

逻辑与单进程相同，扩展到多进程[^1]。

[^1]: 来源：raw/notes/virt/vSVA_logic