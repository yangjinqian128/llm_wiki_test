---
type: knowledge-node
tags: [core-concept, auto-generated, pci, tool]
last_compiled: 2026-05-22
---

# lspci/setpci

[[lspci]]和[[setpci]]是PCIe用户态调试工具[^1]。

## lspci用法

| 命令 | 功能 |
|------|------|
| `lspci` | 显示设备列表[^1] |
| `lspci -t` | 显示拓扑树[^1] |
| `lspci -s BDF` | 显示指定设备[^1] |
| `lspci -vvv` | 显示详细信息[^1] |
| `lspci -xxx` | 显示配置空间十六进制[^1] |

## BDF解析

`lspci -t`输出：
- `0000:00`：domain 0, bus 0[^1]
- `01.0-[01]`：device 01.0, subordinate bus 01[^1]
- `[01]----00.0`：bus 01上的device 00.0[^1]

## setpci用法

```bash
setpci -s BDF 20.L=0x12345678
```

| 后缀 | 位宽 |
|------|------|
| B | 8bit[^1] |
| W | 16bit[^1] |
| L | 32bit[^1] |

## Sysfs接口

| 文件 | 功能 |
|------|------|
| `remove` | 移除设备[^1] |
| `rescan` | 重扫描设备[^1] |
| `reset` | 复位设备[^1] |

## 相关概念

- [[PCIe配置空间]] - 配置空间访问
- [[FLR]] - 设备复位
- [[PCIe热插拔]] - 热插拔操作

[^1]: 来源：raw/notes/pci/lspci_setpci_note