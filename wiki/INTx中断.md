---
type: knowledge-node
tags: [core-concept, auto-generated, pci, interrupt]
last_compiled: 2026-05-22
---

# INTx中断

[[INTx中断]]是PCIe的传统中断机制（INTA~INTD）[^1]。

## DTS配置解析

ARM架构下INTx中断通过DTS配置[^1]：

```c
pci_device_add()
  → pcibios_add_device(dev)
      → dev->irq = of_irq_parse_and_map_pci(dev, 0, 0)
          → of_irq_parse_and_map_pci()
              → pci_device_to_OF_node(pdev)
              → pci_read_config_byte(pdev, PCI_INTERRUPT_PIN, &pin)
              → of_irq_parse_raw  // 解析interrupt-cell, interrupt-map
          → irq_create_of_mapping()
```
[^1]

## 中断号分配

分配的virq写入`pci_dev->irq`，供驱动注册handler[^1]。

## 共享中断

多个设备可能共享一个INTx线，handler需shareable[^1]。

中断触发时所有共享handler都会被调用[^1]。

## of_node设置

`pci_scan_device`通过`pci_set_of_node()`设置`pci_dev->of_node`[^1]。

## 相关概念

- [[MSI]] - 消息信号中断
- [[GIC中断控制器]] - ARM中断控制器
- [[中断处理]] - 中断处理机制

[^1]: 来源：raw/notes/pci/pci_intx_study