---
title: ACPI-MADT
aliases: [MADT, Multiple APIC Description Table]
tags: [ACPI, ARM, GIC, 中断控制器]
source: raw/notes/madt_note
---

# ACPI-MADT

Multiple APIC Description Table，定义 CPU core 和中断控制器信息。

## 表功能

- 定义 CPU core 信息（MPIDR、CPU ID）
- 定义 GICR/GICD/ITS 等中断控制器

## ARM Subtable 类型

| Type | 说明 |
|------|------|
| 0x0B | GICR（每个 core 一个） |
| 0x0C | GICD |
| 0x0F | ITS |

## GICR 结构

每个 core 一个 GICR，包含：

- MPIDR（ARM64 处理器 ID）
- CPU ID（逻辑 CPU 编号）
- GICR 物理地址

## ITS 与 IORT 关联

IORT 中 ITS 节点的 Identifiers = MADT 中 ITS 节点标号。

通过此关联定义 PCIe 设备 MSI/MSI-X 中断上报到哪个 ITS。

## dump 方法

从系统 dump 的 MADT 表为 `apic.dsl`（沿用 X86 表名）。

## 相关概念

- [[ACPI表结构解析]] - ACPI 表总览
- [[ACPI-PPTT]] - CPU/Cache 拓扑表
- [[GIC中断控制器]] - ARM GIC
- [[ITS]] - Interrupt Translation Service

---

**参考来源**：[madt_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/madt_note)