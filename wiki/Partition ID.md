---
title: Partition ID
tags: [ARM, MPAM, 资源管理]
created: 2026-05-22
source: raw/notes/ARM/MPAM基本逻辑分析.md
---

# Partition ID

MPAM三元组中的分区标识，用于区分不同的资源分区。

## 概念

Partition ID是MPAM全局唯一三元组的元素之一：
- 配合Partition ID space和PMG标记内存访问源头
- 用于资源控制和监控的区分

## PE配置

对于PE，partition ID需配置到`MPAMx_ELn`系统寄存器：
- 若partition ID语义是PE本身，配置一次即可
- 若partition ID表示线程，需在线程切换时更新配置

## SMMU配置

SMMU上的STE/CD可看作外设在SMMU上的代理：
- 只需把partition ID配置到SMMU
- 联合内存或cache上的控制单元实现资源控制

## resctrl绑定

用户通过resctrl文件系统：
- 创建目录对应partition ID
- 通过`cpus/cpus_list/tasks`文件绑定CPU或线程

## Partid Narrow

进入MSC的partition ID可通过提前配置的映射被映射成另外一个partition ID：
- 后续控制和检测都基于新partition ID
- 用于partition ID预留或虚拟化场景

## 虚拟化

虚拟化需要：
1. partition ID预留
2. 虚拟物理partition ID映射

## 相关概念

- [[MPAM]]
- [[MSC]]
- [[PMG]]
- [[resctrl文件系统]]

[^1]: 来源: raw/notes/ARM/MPAM基本逻辑分析.md