---
type: knowledge-node
tags: [core-concept, auto-generated, ARM64, kvmtool]
last_compiled: 2026-05-22
---

# kvmtool地址布局

kvmtool ARM 平台的 Guest 物理地址空间布局[^1]。

## 内存布局

```
0      64K  16M     32M     48M            1GB       2GB
+-------+----+-------+-------+--------+-----+---------+---...
|  PCI  |////| plat  |       |        |     |         |
|  I/O  |////| MMIO: | Flash | virtio | GIC |   PCI   |  DRAM
| space |////| UART, |       |  MMIO  |     |  (AXI)  |
|       |////| RTC,  |       |        |     |         |
|       |////| PVTIME|       |        |     |         |
+-------+----+-------+-------+--------+-----+---------+---...
```

## 关键常量

| 区域 | 基址 | 大小 |
|------|------|------|
| PCI I/O | 0x0 | 64K |
| 平台 MMIO | 16M | - |
| Flash | 32M | 16M |
| Virtio MMIO | 48M | - |
| GIC Distributor | 1G - 64K | 64K |
| PCI ECAM | 1G | 256M |
| PCI MMIO | 1.25G | 768M |
| **DRAM** | **2G** | - |

## 设计要点

- 低 2G 全部留给 MMIO[^1]
- DRAM 从 2G 开始[^1]
- GIC 紧贴 1G 下方[^1]
- AArch32 最大 RAM 为 2G[^1]

## 相关概念

- [[KVM]]
- [[GIC中断控制器]]
- [[PCIe]]

[^1]: 来源: raw/notes/ai_gen/kvmtool_ARM地址空间定义.md