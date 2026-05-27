---
title: ITS (Interrupt Translation Service)
tags: [ARM, GIC, 中断]
created: 2026-05-22
source: raw/notes/ARM/gic/gic_v3.md
---

# ITS (Interrupt Translation Service)

GICv3架构中的中断转换服务，实现中断ID到中断路由的映射。

## 功能

ITS的核心功能：
- 维持中断标识的不变性（invariant）
- 实现DeviceID + EventID到InterruptID的转换
- 支持中断重定向和负载均衡

## 工作原理

中断转换流程：
1. 外设发送MSI：包含DeviceID和EventID
2. ITS查找转换表：DeviceID + EventID -> InterruptID
3. ITS根据InterruptID路由到目标CPU的Redistributor

## 转换表

ITS维护的转换表：
- **Device Table**: DeviceID到Collection映射
- **Interrupt Translation Table**: EventID到InterruptID/LPI映射
- **Collection Table**: Collection到目标CPU映射

## LPI支持

ITS主要用于LPI (Locality-specific Peripheral Interrupts):
- LPI数量远大于SPI（可达数万）
- 支持动态分配和路由
- LPI配置表存储在内存中

## 虚拟化支持

ITS支持虚拟化：
- 虚拟ITS可以模拟中断转换
- 支持虚拟LPI注入
- 虚拟机可直接接收MSI

## 命令队列

ITS通过命令队列接收命令：
- ITS命令写入内存中的命令队列
- ITS读取并执行命令
- 支持批量命令处理

典型命令：
- INT: 映射DeviceID + EventID到InterruptID
- MOVI: 移动中断到不同Collection
- SYNC: 同步命令执行
- DISCARD: 丢弃中断

## 相关概念

- [[GICv3]]
- [[GIC中断控制器]]
- [[LPI]]
- [[MSI]]

[^1]: 来源: raw/notes/ARM/gic/gic_v3.md