---
type: knowledge-node
tags: [core-concept, auto-generated, arm-iommu]
last_compiled: 2026-05-22
---

# SMMU

[[SMMU]]（System Memory Management Unit）是 ARM 架构的 [[IOMMU]] 实现[^1]。

## 版本

[[SMMUv3]] 是当前主流版本，支持两级地址翻译（Stage-1 和 Stage-2）[^1]。

## BBML 同步机制

[[BBML]]（Break Before Make Level）处理页表修改时的同步问题[^1]。

页表修改可能与硬件访问并发，需要同步机制[^1]。

### Level 0

软件完全保证同步，通过加锁、清页表、[[TLBI]] 实现[^1]。

### Level 1

大页拆小页流程：
1. 设置 block 页表的 nT bit（禁止 TLB 缓存）[^1]
2. 执行 [[TLBI]] 清除 TLB[^1]
3. 原子替换为散开的 page 页表[^1]

### Level 2

无需 nT bit，硬件在地址多 TLB 时选择一个使用[^1]。

[[BBML2]] 详细分析见 CPU/SMMU 对比文档。

## HTTU

[[HTTU]] (Hardware Translation Table Update) 硬件自动更新页表[^2]。

- Hardware Access flag (HA)
- Hardware Dirty (HD) - [[脏页跟踪]]支持

## SVA 支持

[[SVA依赖BBML2]] - SVA 安全工作的硬性前提[^2]。

## 脏页跟踪

[[SMMU脏页跟踪]] - VFIO 设备 DMA 内存追踪[^2]。

## TLB管理

[[TLBI]]（TLB Invalidate）无效化TLB条目[^2]。

支持Range Invalidate和Context Invalidate[^2]。

## 性能监控

[[SMMU-PMU]]（PMCG）提供性能统计[^3]。

可监控tlb_miss、transaction、cycles等事件[^3]。

## 适用场景

- **内核 DMA**：不可 stop，页表一般不变[^1]
- **[[SVA]]**：可 stop，进入 fault 流程[^1]
- **热迁移**：大页拆小页时需变动页表[^1]

## Fault处理

[[IOPF]]处理设备DMA访问时的缺页[^4]。

[^1]: 来源：raw/notes/iommu/smmu_bbml
[^2]: 来源：raw/notes/iommu/smmu_tlbi_note
[^3]: 来源：raw/notes/iommu/smmu_pmu
[^4]: 来源：raw/notes/iommu/iopf_timer