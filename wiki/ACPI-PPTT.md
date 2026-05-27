---
title: ACPI-PPTT
aliases: [PPTT, Processor Properties Topology Table]
tags: [ACPI, ARM, CPU拓扑, Cache]
source: raw/notes/pptt_note
---

# ACPI-PPTT

Processor Properties Topology Table，定义 CPU 核和 Cache 层次信息。

## 表功能

- 定义 CPU 核层次：thread → core → die → cluster → socket → system
- 定义 Cache 与 CPU 核层级关系

## 层次结构

```
socket ←── L3
    ↑
cluster
    ↑         ←── L1
core      ←── L2
 ↑   ↑
 │   └─→ thread
 └─→ thread
```

## 节点定义

每个层级占一个节点，通过 `parent` 域段从叶子指向父节点。

Cache 节点的 `parent` 指向对应 CPU 核层级节点。

## 相关概念

- [[ACPI表结构解析]] - ACPI 表总览
- [[ACPI-MADT]] - CPU/GIC 定义表
- [[Cache]] - Cache 结构
- [[NUMA]] - NUMA 拓扑

---

**参考来源**：[pptt_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/pptt_note)