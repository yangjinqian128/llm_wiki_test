---
type: knowledge-node
tags: [core-concept, auto-generated, pci, error]
last_compiled: 2026-05-22
---

# AER

[[AER]]（Advanced Error Reporting）是PCIe的高级错误报告机制[^1]。

## 架构

AER作为PCIe Port Service注册[^1]：

```
pcie_portdrv_init
  → pcie_port_bus_register
  → pci_register_driver
      → pcie_port_device_register
          → get_port_device_capability  // 检测AER支持
          → init_service_irqs           // 分配中断
          → pcie_device_init            // 创建pcie_device
```
[^1]

## AER驱动

```
aer_service_init
  → pcie_port_service_register(&aerdriver)
      → aer_probe
          → aer_alloc_rpc
          → request_irq
          → aer_enable_rootport
```
[^1]

## 错误报告

通过ftrace报告错误[^1]：

```c
cper_print_aer
  → trace_aer_event
```
[^1]

## 中断配置

PCIe spec要求使用MSI/MSI-X/INTx作为中断[^1]。

部分SoC使用MBIGEN专用中断[^1]。

## VF直通场景

PF发生AER错误时，扫描父总线所有function，调用驱动的`err_detected`回调[^2]。

VFIO VF的`vfio_pci_aer_err_detected`向QEMU发送eventfd，QEMU终止进程[^2]。

## 相关概念

- [[DPC]] - 错误containment
- [[FLR]] - 错误后复位
- [[RAS]] - 系统可靠性
- [[VFIO]] - VF错误处理

[^1]: 来源：raw/notes/pci/AER_note
[^2]: 来源：raw/notes/pci/pci_ras_logic