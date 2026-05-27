---
title: PASID结构设计
aliases: [pasid, PASID数据结构, DevMMU PASID]
tags: [IOMMU, PASID, SVA, qemu]
source: raw/notes/pasid_struct
---

# PASID结构设计

PASID（Process Address Space ID）相关的软硬件设计。

## QEMU PASID 支持思路

新增数据结构存放 PASID 和 DevMMUHandle，传递给 devmmu_translate 函数。

## PCIe PASID Capability

PASID 是 PCIe 扩展空间 cap，位于 0x100 开始。

QEMU 模拟 PCIe 设备需通过 PCIe Root Port：

```bash
-device pcie-root-port,id=root_port,bus=pcie.0 \
-device ghms_pci,bus=root_port
```

## 内核 DevMMU 数据结构

| 结构 | 说明 |
|------|------|
| devmmu_master | 服务一个 EP 设备 |
| devmmu_bond | 设备与进程绑定关系（一个 PASID） |
| devmmu_mmu_notifier | mm 注册的 notifier，一个 mm 对应多个 bond |

## Bond 与 ASID

多个设备共用进程地址空间时，多个 bond 对应一个 devmmu_mmu_notifier，TLBI 用同一个 ASID。

## 遗留问题

1. Device tab 处理 ASID 时不能同时用 kernel DMA
2. 第二次带 PASID DMA，mmap mmio 时卡在 do_fault 锁

## 相关概念

- [[PASID]] - PASID 概念
- [[SVA]] - Shared Virtual Addressing
- [[IOMMU]] - IOMMU 框架
- [[UACCE]] - 加速器框架

---

**参考来源**：[pasid_struct](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/pasid_struct)