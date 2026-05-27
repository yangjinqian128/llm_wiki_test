---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization, iommu]
last_compiled: 2026-05-22
---

# iommufd

[[iommufd]]是Linux内核新的IOMMU用户态接口框架[^1]。

## 基本逻辑

[[iommufd]]取代VFIO container实现[^1]。

内核配置：`VFIO_DEVICE_CDEV`[^1]。

## 字符设备路径

`/dev/vfio/devices/vfioN`[^1]。

VF设备绑定VFIO驱动时创建新字符设备[^1]。

## 核心操作

1. **open vfio cdev**：返回VF设备fd[^1]
2. **open /dev/iommu**：返回iommufd fd[^1]
3. **VF cdev和iommufd绑定**：返回devid[^1]
4. **alloc ioas**：首次创建containers[^1]
5. **alloc hwpt**：硬件页表分配[^1]
6. **attach hwpt**：绑定硬件页表[^1]

## QEMU集成

```
vfio_realize -> vfio_attach_device -> iommufd_cdev_attach
```
[^1]

## DMA标脏

支持[[DMA内存标脏]][^1]。

依赖SMMU [[HTTU]]特性[^1]。

## vSMMU支持

```
iommufd_backend_alloc_viommu -> create vSMMU
```
[^1]

## 相关概念

- [[VFIO]] - 设备直通框架
- [[vSVA]] - 虚拟化SVA
- [[vSMMU]] - 虚拟SMMU
- [[IOMMU]] - 地址翻译硬件

[^1]: 来源：raw/notes/virt/基于iommufd的vSMMU基本逻辑分析