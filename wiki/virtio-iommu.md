---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# virtio-iommu

[[virtio-iommu]]是纯软件的IOMMU方案[^1]。

## 实现位置

QEMU：`hw/virtio/virtio-iommu.c`[^1]。

Guest内核：`drivers/iommu/virtio-iommu.c`[^1]。

## 与nested IOMMU区别

[[virtio-iommu]]用纯软件实现VA→IPA映射[^1]。

nested IOMMU/SMMU使用物理硬件[^1]。

## PASID支持

virtio-iommu spec中PASID/fault支持不完善[^1]。

## 协议规范

jpbrucker.net/virtio-iommu/spec[^1]。

## 硬件支持扩展

可基于VFIO接口实现物理SMMU支持[^1]。

需要virtio-iommu协议支持[^1]。

## 相关概念

- [[vSMMU]] - 虚拟SMMU
- [[vSVA]] - 虚拟化SVA
- [[VFIO]] - 设备直通

[^1]: 来源：raw/notes/virt/vSVA_logic