---
title: ACPI-PCI配置
created: 2026-05-22
tags:
  - ACPI
  - PCI
  - PCIe
---

# ACPI-PCI配置

## 概述

ACPI表中定义了PCI/PCIe主机桥的配置方法，支持静态配置和热插拔配置。

## MCFG表

- **用途**：非热插拔PCI配置空间
- **内容**：ECAM(Enhanced Configuration Access Mechanism)区域基地址

## 热插拔支持

- **_CBA**：热插拔场景下的配置空间基地址方法

## ECAM空间

- 可分布在系统的多个host bridge
- 大小由起始和结束总线号定义（0~255）
- MCFG直接引用_SEG定义的PCI Segment Group

## ACPI命名空间对象

### 设备标识

- **PNP0A03**：PCI host bridge
- **PNP0A08**：PCIe host bridge
- **_CID**：兼容ID，可设置`_CID = PNP0A03`兼容旧OS

### 配置方法

- **_BBN**：Boot Bus Number
- **_SEG**：PCI Segment Group
- **_PRT**：PCI Routing Table，配置INTx
- **_OSC**：OS Capabilities
- **_DSM**：Device Specific Method

## 概念辨析

### Host Bridge vs Root Port

- **Host Bridge**：连接CPU和PCI总线域
- **Root Port**：PCIe交换结构的上游端口

参见[[PCIe概述]]、[[ACPI概述]]。

---

**参考来源**：[acpi_pci](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/acpi/acpi_pci)