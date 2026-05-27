---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# VFIO

[[VFIO]]（Virtual Function I/O）是 Linux 内核驱动，将设备安全地暴露给用户态[^1]。

## 核心功能

支持三个子特性[^1]：
1. **CFG/MEM/IO 访问**：用户态可访问 VF 的配置空间、内存和 IO[^1]
2. **DMA 支持**：VF 数据可翻译到用户态 DMA 内存范围[^1]
3. **中断路由**：VF 中断可路由到 VM[^1]

## 架构层次

- `/dev/vfio/vfio`：[[VFIO container]]，表示多个设备共享的地址空间[^1]
- `/dev/vfio/<group_number>`：[[VFIO group]]，表示共享 [[IOMMU]] 的设备组[^1]
- 打开 group 文件得到 fd，表示单个设备[^1]

## 设备绑定

需解绑原驱动，绑定 [[VFIO]] 设备驱动[^1]。

[[VFIO]] PCI 驱动成为设备代理，导出所有资源到用户态[^1]。

## 数据结构

全局 `vfio` 包含[^1]：
- `group_list`：管理 [[VFIO group]]
- `device_list`：管理 [[VFIO device]]
- `iommu_drivers_list`：管理 [[VFIO IOMMU driver]]

## Container 作用

多个 [[SMMU]] 通过 container 拥有相同映射[^1]。

映射在 container 中统一维护[^1]。

## 设备热插拔

VF通过[[VFIO]]直通时，可支持[[PCIe热插拔]][^2]。

需挂接支持热插拔的PCI桥（如ioh3420）[^2]。

## 错误处理

PF发生[[AER]]错误时，VFIO的`err_detected`向QEMU发送eventfd[^3]。

## 复位机制

VFIO可通过[[FLR]]复位设备[^4]。

enable/disable和ioctl（`VFIO_DEVICE_RESET`）触发FLR[^4]。

## iommufd替代

[[iommufd]]逐步取代VFIO container功能[^5]。

新的字符设备路径：`/dev/vfio/devices/vfioN`[^5]。

## vSVA支持

[[vSVA]]依赖VFIO配置S2翻译[^6]。

## 热迁移

[[热迁移]]带VFIO设备需enable-migration=on[^7]。

[[SMMU脏页跟踪]] - 设备 DMA 内存脏页追踪。

## dma-buf 集成

[[dma-buf]] - 跨设备共享缓冲区。

[[dma_fence]] - DMA 完成同步。

[[dma_resv]] - Fence 管理。

## 大页映射

[[iommu_pgsize]] - 决定 IOMMU 映射粒度。

## 相关概念

- [[PCIe热插拔]] - VF热插拔
- [[AER]] - 错误处理
- [[FLR]] - 设备复位
- [[ACS]] - 设备隔离
- [[iommufd]] - 新IOMMU接口
- [[vSVA]] - 虚拟化SVA
- [[vSMMU]] - 虚拟SMMU
- [[热迁移]] - 虚拟机迁移
- [[SR-IOV]] - SR-IOV虚拟化
- [[eventfd]] - 中断通知机制

[^1]: 来源：raw/notes/virt/vfio/vfio_note
[^2]: 来源：raw/notes/pci/pci_dev_hotplug_in_qemu
[^3]: 来源：raw/notes/pci/pci_ras_logic
[^4]: 来源：raw/notes/pci/PCI_FLR_note
[^5]: 来源：raw/notes/virt/基于iommufd的vSMMU基本逻辑分析
[^6]: 来源：raw/notes/virt/vSVA_logic
[^7]: 来源：raw/notes/virt/qemu_cmdline