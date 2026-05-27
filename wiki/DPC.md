---
type: knowledge-node
tags: [core-concept, auto-generated, pci, error]
last_compiled: 2026-05-22
---

# DPC

[[DPC]]（Downstream Port Containment）在检测到错误时隔离下游端口[^1]。

## 驱动注册

作为PCIe Port Service注册（port_type = PCIE_ANY_PORT）[^1]。

## Probe流程

```c
dpc_probe
  → INIT_WORK(&dpc->work, interrupt_event_handler)
  → devm_request_irq(..., dpc_irq, ...)
  → pci_write_config_word(pdev, dpc->cap_pos + PCI_EXP_DPC_CTL, ctl)
      // 使能non-fatal中断
```
[^1]

## 中断处理

```c
dpc_irq
  → pci_read_config_word(DPC_STATUS)
  → pci_read_config_word(DPC_SOURCE_ID)
  → schedule_work(&dpc->work)
```
[^1]

## 错误Containment

```c
interrupt_event_handler
  → pci_stop_and_remove_bus_device(dev)  // 递归移除下游设备
      → pci_stop_bus_device(dev)
          → pci_stop_dev(dev)  // 调用PME/ASPM函数
      → pci_remove_bus_device(dev)
          → pci_destroy_dev(dev)
  → dpc_wait_link_inactive(pdev)  // 等待最多1秒
  → pci_write_config_word(DPC_STATUS, TRIGGER | INTERRUPT)
```
[^1]

## 设备移除顺序

从最深层设备开始，逐一向上移除[^1]。

## 相关概念

- [[AER]] - 错误检测
- [[PCIe热插拔]] - 设备移除机制
- [[FLR]] - 错误后复位
- [[RAS]] - 系统可靠性

[^1]: 来源：raw/notes/pci/pcie_dpc