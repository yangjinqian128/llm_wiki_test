---
type: knowledge-node
tags: [core-concept, auto-generated, iommu]
last_compiled: 2026-05-22
---

# IOMMU

[[IOMMU]]（Input/Output Memory Management Unit）是外设的 MMU，为设备 DMA 操作提供地址翻译和权限管理[^1]。

## 核心功能

[[IOMMU]] 提供：
- 地址翻译：将离散物理地址映射到连续虚拟地址空间[^1]
- 权限管理：保证外设访问系统内存的安全性[^1]
- 地址共享：使 CPU 和设备看到相同的虚拟地址空间[^1]

## SMMU 硬件架构

[[SMMU]] 使用三个队列和两个表格：
- **STE**（Stream Table Entry）：描述一个外设的内存访问信息，通过 [[SID]] 关联[^1]
- **CD**（Context Descriptor）：描述设备的独立内存访问单元，通过 [[SSID]]/[[PASID]] 关联[^1]
- **Command Queue**：软件向 SMMU 发送命令[^1]
- **Event Queue**：SMMU 向软件报告异常[^1]
- **PRI Queue**：处理 PCIe 设备的页请求[^1]

## 缓存机制

[[SMMU]] 包含 TLB、STE cache 和 CD cache 以加速翻译和查找[^1]。

[[TLBI]] 无效化TLB条目。

## Fault处理

[[IOPF]] 处理设备DMA缺页。

## 性能监控

[[SMMU-PMU]] 提供性能统计。

## Linux 驱动架构

核心数据结构：
- `struct arm_smmu_device`：描述物理 SMMU 设备[^1]
- `struct arm_smmu_master`：描述 SMMU 管理的外设[^1]
- `struct iommu_domain`：打包地址翻译类型[^1]

## 关键特性

- [[BBML2]] - 页表修改同步机制
- [[HTTU]] - 硬件自动更新页表
- [[SMMU脏页跟踪]] - DMA内存脏页追踪
- [[iommu_pgsize]] - 映射粒度选择
- [[IO页表]] - 设备地址翻译

## DMA共享

- [[dma-buf]] - 跨设备共享缓冲区
- [[dma_fence]] - DMA完成同步
- [[dma_resv]] - Fence管理

## 虚拟化集成

- [[VFIO]] - VFIO 框架使用 IOMMU 进行设备直通
- [[vSVA]] - 虚拟化 SVA 支持
- [[热迁移]] - 设备迁移依赖 IOMMU 状态
- [[SR-IOV]] - SR-IOV 虚拟化与 IOMMU

---
[^1]: 来源：raw/notes/iommu/iommu_call_flow