---
title: PMG (Performance Monitor Group)
tags: [ARM, MPAM, 性能监控]
created: 2026-05-22
source: raw/notes/ARM/MPAM基本逻辑分析.md
---

# PMG (Performance Monitor Group)

MPAM三元组中的性能监控组标识，用于聚集一组监测资源。

## 概念

PMG是MPAM全局唯一三元组的元素之一：
- 配合Partition ID space和Partition ID标记内存访问源头
- 用于聚集一组需要监控的资源

## 配置

对于PE，PMG需配置到`MPAMx_ELn`系统寄存器，与Partition ID一起设置。

## 监控流程

MSC中某种类型的monitor:
1. 配置`MSMON_CFG_MON_SEL.RIS`选择type
2. 配置`MSMON_CFG_MON_SEL.MON_SEL`选择instance
3. 配置`MSMON_CFG_*_FLT.PARTID/PMG`选择partid+PMG
4. 配置`MSMON_CFG_*_CTL`启动监控
5. 读`MSMON_*`counter得到监控数据

## 监控寄存器

过滤寄存器配置PARTID/PMG：
- `MSMON_CFG_CSU_FLT`: cache使用监控过滤
- `MSMON_CFG_MBWU_FLT`: 内存带宽监控过滤

硬件过滤：partid和PMG都一样才能过滤。

## Monitor实例

一个MSC中某种类型可有多个monitor counter（monitor instance），数量在ID寄存器中定义：
- `MPAMF_MBWUMON_IDR.NUM_MON`: memory bandwidth monitor实例数
- `MPAMF_CSUMON_IDR.NUM_MON`: cache usage monitor实例数

## 相关概念

- [[MPAM]]
- [[MSC]]
- [[Partition ID]]

[^1]: 来源: raw/notes/ARM/MPAM基本逻辑分析.md