---
type: knowledge-node
tags: [core-concept, auto-generated, pci-iommu]
last_compiled: 2026-05-22
---

# PASID

[[PASID]]（Process Address Space ID）是 PCIe 协议中用于标识进程地址空间的 ID[^1]。

## 硬件支持

PCIe 设备通过 PASID Extended Capability 表示支持发送/接收带 PASID TLP Prefix 的 TLP[^1]。

## 与 SMMU 的关系

在 [[SMMU]] 中，[[SSID]] 对应 [[PASID]][^1]。

通过 [[PASID]]，设备可以区分不同进程的内存访问请求[^1]。

## 地址翻译链

```
SID -> STE -> SSID/PASID -> CD -> IOVA -> PTE -> IPA/PA
```
[^1]

## SVA 应用

通过 [[iommu_bind_task]] 将进程 mm 绑定到设备[^1]。

绑定后，设备与 CPU 共享同一地址空间，需要通过 [[mm_notifier]] 同步页表变化[^1]。

## I/O缺页处理

带PASID的DMA访问可触发[[IOPF]]，由软件处理缺页[^2]。

## 性能监控

使用[[Perf trace]]跟踪IOPF事件[^3]。

## ASID 与缓存

[[ASID]] 用于标记 TLB 中的进程，避免上下文切换时刷新全部 TLB[^1]。

## 相关概念

- [[IOPF]] - IO缺页处理
- [[Perf trace]] - 缺页跟踪
- [[SMMU-PMU]] - 性能监控

[^1]: 来源：raw/notes/iommu/PASID_analysis
[^2]: 来源：raw/notes/iommu/iopf_timer
[^3]: 来源：raw/notes/perf/perf_trace