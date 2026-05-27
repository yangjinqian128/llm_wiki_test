---
title: MPAM
tags: [ARM, 架构, 资源管理]
created: 2026-05-22
source: raw/notes/ARM/MPAM基本逻辑分析.md
---

# MPAM (Memory Partition and Monitor)

ARM内存系统资源分区与监控架构，用于对共享内存系统资源（cache、内存带宽等）进行控制和监测。

## 需求背景

现代服务器运行多用户多程序，硬件资源共享。软件可通过cgroup调控CPU、内存等资源，但cache使用、内存带宽使用等硬件资源难以直接调控。ARM引入MPAM对此类资源做控制和监测。

## 三元组标记

MPAM使用全局唯一三元组标记每个内存访问源头：
- **Partition ID space**: 安全态标识，区分资源类型
- **Partition ID**: 分区标识，用于资源区分
- **PMG** (Performance Monitor Group): 性能监控组，用于聚集监测资源

对于PE，需配置到`MPAMx_ELn`系统寄存器。若partition ID语义是PE本身，配置一次即可；若表示线程，需在线程切换时更新。

## MSC (Memory System Component)

MSC是集成在cache/内存控制器/SMMU上的MPAM控制节点：
- 提供MMIO寄存器接口
- 接受三元组的资源控制和监控配置
- 系统运行时根据配置进行资源控制和监控

MSC通过ACPI或DTS MPAM表格报给OS。

## 控制类型

- **capacity-based partitioning**: 动态比较当前使用资源与配置资源，有余量则分配
- **portion-based partitioning**: 直接配置可用cache way等，调整逻辑简单

硬件内部用表格记录资源配置，若部件有低功耗上下电动作，需保存和恢复。

## 中断类型

MPAM只有两种中断：
1. 报告错误的中断
2. monitor计数溢出中断

## 寄存器分类

MPAM软硬件接口寄存器分四类：
1. **ID寄存器**: 配置MSC规格（MPAMF_AIDR、MPAMF_IIDR等）
2. **系统寄存器**: 配置流量源头的partition ID和PMG（MPAMx_ELn）
3. **control寄存器**: 资源控制（MPAMCFG_CASSOC、MPAMCFG_MBW_MAX等）
4. **monitor寄存器**: 资源监控（MSMON_CFG_MON_SEL、MSMON_CSU等）

## SMMU MPAM

SMMU只配置partid，联合内存或cache上的控制单元实现资源控制。STE/CD可看作外设在SMMU上的代理，只需把partid配置到SMMU即可。

## Partid Narrow

进入MSC的partid可通过提前配置好的映射被映射成另外一个partid，后续控制和检测都基于新partid。

## 虚拟化支持

1. partition ID预留
2. 虚拟物理partition ID映射
3. MSC模拟

## Linux接口

使用resctrl文件系统向外导出接口。用户通过resctrl把特定CPU或线程和partid绑定，在各控制节点上配置partid对应的控制和监控信息。

参见: [[resctrl文件系统]]

## 相关概念

- [[MSC]]
- [[Partition ID]]
- [[PMG]]
- [[resctrl文件系统]]

[^1]: 来源: raw/notes/ARM/MPAM基本逻辑分析.md