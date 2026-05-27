---
type: knowledge-node
tags: [core-concept, auto-generated, pci]
last_compiled: 2026-05-22
---

# PCIe配置空间

[[PCIe配置空间]]存储设备的配置信息，通过lspci/setpci读写[^1]。

## 配置空间大小

- PCI设备：256字节[^1]
- PCIe设备：4096字节（`PCI_CFG_SPACE_EXP_SIZE`）[^1]

## Sysfs访问

通过`/sys/bus/pci/devices/<BDF>/config`文件读写[^1]。

内核实现：
```c
static struct bin_attribute pcie_config_attr = {
    .size = PCI_CFG_SPACE_EXP_SIZE,
    .read = pci_read_config,
    .write = pci_write_config,
};
```
[^1]

## lspci实现

`pciutils`使用`pm_linux_sysfs`方法[^1]：

```
lspci → sysfs_read/write
         → open(/sys/bus/pci/devices/<BDF>/config)
         → read/write fd
         → kernel pci_read_config/pci_write_config
```
[^1]

## BAR寄存器

通过`/sys/devices/<device_path>/resource[n]`访问BAR空间[^3]。

使用`pcie_debug`工具交互式读取[^3]。

## 相关概念

- [[ACS]] - 访问控制
- [[FLR]] - 功能复位
- [[AER]] - 错误报告
- [[MSI]] - 中断配置

[^1]: 来源：raw/notes/pci/lspci_read_write_config_space
[^2]: 来源：raw/notes/pci/pci_standard_study
[^3]: 来源：raw/notes/pci/pcie_debug