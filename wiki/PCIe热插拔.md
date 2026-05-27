---
type: knowledge-node
tags: [core-concept, auto-generated, pci]
last_compiled: 2026-05-22
---

# PCIe热插拔

[[PCIe热插拔]]允许在不重启系统的情况下添加或移除PCIe设备[^1]。

## 热插拔类型

- **Native Hotplug**：使用Linux内核原生实现[^1]
- **ACPI Hotplug**：基于ACPI表实现[^1]

## 状态机

| 状态 | 说明 |
|------|------|
| STATIC_STATE | 设备正常运行或已移除 |
| BLINKINGON_STATE | 等待5秒后上电 |
| BLINKINGOFF_STATE | 等待5秒后下电 |
| POWERON_STATE | 正在上电 |
| POWEROFF_STATE | 正在下电 |

## 标准热插入流程

1. 插入设备 → Presence Detect中断[^1]
2. 等待Link稳定[^1]
3. 配置资源（`pci_hp_add_bridge`）[^1]
4. 驱动加载[^1]

## 标准热移除流程

1. 按下Attention Button[^1]
2. Power LED闪烁5秒[^1]
3. 移除设备（`pci_stop_and_remove_bus_device`）[^1]
4. 清除资源[^1]

## QEMU VF热插拔

VF通过[[VFIO]]直通时，需挂接支持热插拔的PCI桥（如ioh3420）[^2]。

Guest内核配置：
- `CONFIG_HOTPLUG_PCI_PCIE=y`
- 启动参数：`pcie_ports=native`[^2]

## 资源预留

内核为热插拔桥预留bus号：
```c
if (bus->self->is_hotplug_bridge && pci_hotplug_bus_size)
    max = bus->busn_res.start + pci_hotplug_bus_size - 1;
```
[^1]

## 相关概念

- [[SR-IOV]] - VF热插拔场景
- [[VFIO]] - 设备直通
- [[ACS]] - 设备隔离
- [[DPC]] - 错误containment

[^1]: 来源：raw/notes/pci/pcie_hotplug
[^2]: 来源：raw/notes/pci/pci_dev_hotplug_in_qemu