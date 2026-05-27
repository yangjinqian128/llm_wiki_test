---
type: knowledge-node
tags: [core-concept, auto-generated, pci-iommu]
last_compiled: 2026-05-22
---

# ATS

[[ATS]]（Access Translation Service）是 PCIe 协议特性，允许设备缓存翻译后的物理地址[^1]。

## 工作原理

启用 [[ATS]] 后，设备预先获取 PA 存储在 ATC（Address Translation Cache）中[^1]。

设备发送带翻译标记的 Memory Read/Write TLP，[[SMMU]] 检测标记后直接转发[^1]。

## 性能优势

避免每次 DMA 操作都经过 [[SMMU]] 翻译[^1]。

## 硬件要求

- PCIe 设备需有 ATS Extended Capability[^1]
- [[SMMU]] 需启用 `IDR0_ATS`[^1]
- STU（Smallest Translation Unit）需匹配 SMMU 页大小[^1]

## ATS 无效化

VA→PA 映射变化时，软件需通知设备清除缓存[^1]。

[[SMMU]] 使用 `CMD_ATC_INV` 命令无效化设备 ATC[^1]。

配合[[TLBI]]同步无效化SMMU TLB和设备ATC[^2]。

## 软件支持

Linux 中 `pci_enable_ats(pdev, stu)` 启用 ATS[^1]。

`arm_smmu_atc_inv_domain` 执行无效化[^1]。

## 性能监控

[[SMMU-PMU]]可监控ATS相关事件[^3]：
- `pcie_ats_trans_passed`
- `pcie_ats_trans_rq`

## 相关概念

- [[TLBI]] - TLB无效化
- [[SMMU-PMU]] - ATS性能监控
- [[TPH]] - TLP处理提示

[^1]: 来源：raw/notes/pci/ATS_analysis
[^2]: 来源：raw/notes/iommu/smmu_tlbi_note
[^3]: 来源：raw/notes/iommu/smmu_pmu