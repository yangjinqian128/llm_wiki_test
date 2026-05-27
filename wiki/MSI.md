---
type: knowledge-node
tags: [core-concept, auto-generated, pci, interrupt]
last_compiled: 2026-05-22
---

# MSI

[[MSI]]（Message Signaled Interrupt）是PCIe的消息信号中断机制[^1]。

## ACPI IORT配置

IORT表存储ITS、[[SMMU]]和RC配置[^1]。

解析流程：
```c
pci_device_add
  → pci_set_msi_domain(dev)
      → pci_dev_msi_domain(dev)
          → iort_get_device_domain(&pdev->dev, rid)
              → iort_dev_find_its_id
                  → iort_find_dev_node  // 找RC节点
                  → irq_find_matching_fwnode
```
[^1]

## RC节点匹配

通过segment值匹配IORT中的PCI_ROOT_COMPLEX节点[^1]。

## 多映射支持

一个RC可配置多个ITS映射[^1]：
```
Input base: 0x0000, ID Count: 0x4000, Output Reference: ITS1
Input base: 0x6000, ID Count: 0x100, Output Reference: ITS2
```
[^1]

## 驱动申请中断

`pci_enable_msi`从`pci_dev`获取irq domain，调用ITS驱动分配中断[^1]。

## Port Service中断

PCIe Port Service（AER、DPC、Hotplug）可使用MSI/MSI-X/INTx[^2]。

部分SoC使用MBIGEN专用中断[^2]。

## 相关概念

- [[INTx中断]] - 传统中断
- [[GIC中断控制器]] - ITS中断
- [[AER]] - AER中断
- [[PCIe热插拔]] - Hotplug中断

[^1]: 来源：raw/notes/pci/pci_acpi_msi
[^2]: 来源：raw/notes/pci/AER_note