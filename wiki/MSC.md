---
title: MSC (Memory System Component)
tags: [ARM, MPAM, 硬件]
created: 2026-05-22
source: raw/notes/ARM/MPAM基本逻辑分析.md
---

# MSC (Memory System Component)

MPAM架构中的内存系统控制节点，集成在cache、内存控制器、SMMU等部件上。

## 功能

MSC提供MMIO寄存器接口，接受三元组（Partition ID space、Partition ID、PMG）的资源控制和监控配置。

系统运行时，访问cache或内存的请求根据提前配置的信息进行资源控制和监控。

## 硬件实现

MSC内部：
- 表格记录各种资源配置
- 动态比较当前使用资源与配置资源
- 若部件有低功耗上下电，需保存恢复配置表格

## 资源类型

一个MSC可能同时支持多种控制和监控：
- cache资源
- 内存带宽资源
- 通过RIS (Resource Instance Selection)标记类型

## 配置流程

1. 用`MPAMCFG_PART_SEL`选择partition ID和RIS
2. 配置对应寄存器设置资源限制

## ID寄存器

MSC的ID寄存器配置规格：
- `MPAMF_AIDR`: 版本信息
- `MPAMF_IIDR`: 厂商信息
- `MPAMF_IDR`: 特性总体支持信息
- `MPAMF_CCAP/CPOR/CSUMON_IDR`: cache相关
- `MPAMF_MBW/MBWUMON_IDR`: memory相关
- `MPAMF_PRI_IDR`: 优先级相关

## Linux驱动

`struct mpam_msc`表示MSC设备，驱动代码在`drivers/platform/mpam/`。

## 相关概念

- [[MPAM]]
- [[Partition ID]]
- [[PMG]]
- [[resctrl文件系统]]

[^1]: 来源: raw/notes/ARM/MPAM基本逻辑分析.md