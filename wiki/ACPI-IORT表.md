---
title: ACPI-IORT表
created: 2026-05-22
tags:
  - ACPI
  - IORT
  - UEFI
  - SMMU
---

# ACPI-IORT表

## 概述

IORT(IO Remapping Table)是ACPI规范中定义的表结构，用于描述IO重映射拓扑，包括SMMU和ITS节点。

## 编译问题

编译带有IORT表的UEFI时，可能遇到编译工具无法识别IORT表中SMMU节点的问题。

## 解决方案

1. 从 https://acpica.org/downloads 下载最新acpica工具
2. 编译安装

```bash
# 编译安装acpica
make
sudo make install
```

## IORT节点类型

- **ITS Group**：中断翻译服务节点
- **Named Component**：命名组件节点
- **PCI Root Complex**：PCI根复杂节点
- **SMMU**：系统MMU节点
- **SMMUv3**：SMMUv3节点

## 与其他ACPI表的关系

参见[[ACPI概述]]，IORT表与[[DMA映射]]、[[SMMU]]配置相关。

---

**参考来源**：[acpi_iort](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/acpi/acpi_iort)