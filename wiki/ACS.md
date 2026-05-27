---
type: knowledge-node
tags: [core-concept, auto-generated, pci, isolation]
last_compiled: 2026-05-22
---

# ACS

[[ACS]]（Access Control Services）提供PCIe设备隔离能力，确保不同设备间无法相互访问[^1]。

## ACS能力位

| 位 | 名称 | 功能 |
|----|------|------|
| V | Source Validation | 检查Request ID是否在合法bus范围[^1] |
| U | Upstream Forwarding | TLP目标地址在RP窗口时转发上游[^1] |
| R | P2P Redirect | P2P流量重定向到上游[^1] |
| C | Completion Blocking | (待补充)[^1] |

## IOMMU Group绑定

ACS影响设备到[[IOMMU]] Group的绑定[^1]：

```c
for (bus = pdev->bus; !pci_is_root_bus(bus); bus = bus->parent) {
    if (pci_acs_path_enabled(bus->self, NULL, REQ_ACS_FLAGS))
        break;  // ACS使能，分配新iommu_group
    ...
}
```
[^1]

## ACS语义

- **ACS使能**：设备独占iommu_group[^1]
- **ACS未使能**：设备可能共享父设备的iommu_group[^1]

## 内核Bug

`pci_acs_enable=1`设置时机过晚（在SMMU驱动加载后），导致ACS cap=ctrl未设置[^1]。

## 硬件实现

即使未在UEFI配置U、R、C位，若RP将TLP转发到[[SMMU]]使用VA→PA映射，实质已提供ACS级别隔离[^1]。

## 相关概念

- [[IOMMU]] - 设备隔离依赖ACS
- [[VFIO]] - 设备直通需ACS支持
- [[SR-IOV]] - VF隔离
- [[PCIe热插拔]] - 热插拔设备隔离

[^1]: 来源：raw/notes/pci/acs_note