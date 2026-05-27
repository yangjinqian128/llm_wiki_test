---
type: knowledge-node
tags: [core-concept, auto-generated, pci, reset]
last_compiled: 2026-05-22
---

# FLR

[[FLR]]（Function Level Reset）提供PCIe设备的功能级复位[^1]。

## 硬件接口

- **Device Capability**：FLR Capability bit（表示是否支持）[^1]
- **Device Control**：BCR_FLR bit（写触发FLR）[^1]

## 检测支持

```c
pcie_has_flr(struct pci_dev *dev)
```
[^1]

## 触发接口

```c
pci_reset_function()          // 同步复位
pci_reset_function_locked()   // 持锁复位
pci_try_reset_function()      // 尝试复位
    → __pci_reset_function_locked()
        → pcie_flr()
```
[^1]

## Sysfs触发

```bash
echo 1 > /sys/bus/pci/devices/<BDF>/reset
```
[^1]

## VFIO触发

VFIO设备的enable/disable和ioctl（`VFIO_DEVICE_RESET`）触发FLR[^1]。

## 复位流程

```
reset_prepare → FLR操作 → reset_done
```
[^1]

回调函数在`pci_driver->pci_error_handlers`中定义[^1]。

## 注意事项

FLR不销毁软件结构，设备在lspci中仍然可见[^1]。

PF FLR时会通知VF，VF驱动需配合恢复硬件配置[^1]。

## 相关概念

- [[AER]] - 错误报告
- [[DPC]] - 错误containment
- [[VFIO]] - VF复位
- [[SR-IOV]] - VF复位

[^1]: 来源：raw/notes/pci/PCI_FLR_note