---
type: knowledge-node
tags: [core-concept, auto-generated, pci-virtualization]
last_compiled: 2026-05-22
---

# SR-IOV

[[SR-IOV]]（Single Root I/O Virtualization）允许单个 PCIe PF 创建多个 VF[^1]。

## 基本概念

- **PF**（Physical Function）：完整的 PCIe 功能[^1]
- **VF**（Virtual Function）：轻量级 PCIe 功能[^1]

每个 VF 有独立的 BDF 和配置空间[^1]。

可视为独立的网络端口[^1]。

## 直通架构

```
guest(qemu/vfio) <---> host(kvm/vfio/iommu) <---> PCIe(PF/VF)
```
[^1]

## 中断流程

VF 中断路由[^1]：
1. 中断到达 VFIO MSI/MSI-X 部分[^1]
2. 通过 [[eventfd]] 发送给 [[KVM]] ITS[^1]
3. QEMU VFIO 代码预先建立 eventfd 关系[^1]

## 使用场景

将 PF/VF 分配给不同 guest[^1]。

确保数据和中断流程正常[^1]。

## VF热插拔

VF支持[[PCIe热插拔]]，通过sysfs控制`/sys/.../sriov_numvfs`[^2]。

Guest需配置`CONFIG_HOTPLUG_PCI_PCIE`和`pcie_ports=native`[^2]。

## 设备隔离

VF需[[ACS]]支持确保隔离[^3]。

## 错误处理

VF错误通过[[AER]]机制处理[^4]。

## 复位机制

VF可通过[[FLR]]功能级复位[^5]。

## 相关概念

- [[PCIe热插拔]] - VF热插拔
- [[VFIO]] - 设备直通
- [[ACS]] - 设备隔离
- [[AER]] - 错误报告
- [[FLR]] - 功能复位

[^1]: 来源：raw/notes/virt/sr-iov_note
[^2]: 来源：raw/notes/pci/pci_dev_hotplug_in_qemu
[^3]: 来源：raw/notes/pci/acs_note
[^4]: 来源：raw/notes/pci/AER_note
[^5]: 来源：raw/notes/pci/PCI_FLR_note