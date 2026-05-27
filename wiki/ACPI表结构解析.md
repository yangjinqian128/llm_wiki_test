---
title: ACPI表结构解析
created: 2026-05-22
tags:
  - ACPI
  - MCFG
  - DSDT
  - IORT
---

# ACPI表结构解析

## 概述

PCI ACPI涉及三个主要表：DSDT、MCFG和IORT。

## 表结构关系

```
MCFG: 配置ECAM配置空间地址
  |
  |_CBA: MMCONFIG base address

DSDT: 配置IO/MEM范围和SoC特定信息
  |
  |_CRS: MEM/IO space, bus range
  |
  |_PRT: PCI routing table for INTx

IORT: RID, stream ID, device ID映射
```

## MCFG表结构

```
{
    EFI_ACPI_5_0_MCFG_TABLE_CONFIG
    {
        EFI_ACPI_DESCRIPTION_HEADER
        UINT64 Reserved1
    }
    EFI_ACPI_5_0_MCFG_CONFIG_STRUCTURE[]
    {
        Base Address         // ECAM基地址
        Segment Group Number // PCI段组号
        Start Bus Number     // 起始总线号
        End Bus Number       // 结束总线号
        Reserved
    }
}
```

## DSDT设备节点

```
Device (PCI0)
{
    Name (_HID, "HISI0080")    // PCIe Root Bridge
    Name (_CID, "PNP0A03")     // Compatible ID
    Name (_SEG, 0)             // Segment
    Name (_BBN, 0)             // Base Bus Number
    Name (_CCA, 1)             // Cache Coherence
    Method (_CRS, 0, Serialized) {
        Name (RBUF, ResourceTemplate () {
            WordBusNumber (...)   // Bus范围
            QWordMemory (...)     // 64-bit BAR窗口
            QWordIO (...)         // IO窗口
        })
        Return (RBUF)
    }
}
```

## IORT节点类型

| Type | 说明 |
|------|------|
| 0 | ITS Group |
| 1 | Named Component |
| 2 | Root Complex |

### IORT映射示例

```
Input base: 00000000
ID Count: 00002000        // ID数量
Output Base: 00000000
Output Reference: 00000034 // 指向ITS节点
```

## 内核解析流程

```
acpi_init
  +-> acpi_bus_init
    +-> pci_mmcfg_late_init     // 解析MCFG
      +-> pci_mcfg_parse
    +-> iort_table_detect       // 解析IORT
    +-> acpi_scan_init
      +-> acpi_pci_root_init
      +-> acpi_bus_scan
        +-> acpi_bus_attach
          +-> acpi_pci_root_add
            +-> pci_acpi_scan_root
              +-> acpi_pci_root_create
                +-> acpi_pci_probe_root_resources  // 解析_CRS
```

参见[[ACPI-IORT表]]、[[ACPI-PCI配置]]。

---

**参考来源**：[acpi_note_2](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/acpi/acpi_note_2)