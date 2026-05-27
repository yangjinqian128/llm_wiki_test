---
title: GICv3
tags: [ARM, 中断控制器, GIC]
created: 2026-05-22
source: raw/notes/ARM/gic/gic_v3.md
---

# GICv3

ARM Generic Interrupt Controller架构第三版，引入系统寄存器接口和分布式中断处理。

## 核心特性

### 系统寄存器接口

GICv3支持系统寄存器接口：
- 中断控制器寄存器可直接通过系统寄存器访问
- 无需内存映射方式访问
- 提高性能和访问效率

### 中断标识

中断标识应保持不变性（invariant），这导致引入ITS（Interrupt Translation Service）:
- ITS实现中断ID到中断路由的转换
- 支持中断重定向和负载均衡

### Distributor与Redistributor

- SPI (Shared Peripheral Interrupts)连接到Redistributor
- Distributor、Redistributor和ITS都支持虚拟化

### 中断状态

- Active状态与DIR (Deactivation)机制
- 支持中断优先级管理
- 支持中断分组（Group 0/Group 1）

### MSI支持

Redistributor可直接接收MSI/MSI-X，无需ITS帮助:
- 简化MSI中断配置
- 支持per-CPU MSI

### MBI中断

MBI (Message Based Interrupt)可报告为SPI:
- 兼容传统SPI处理方式
- 灵活的中断类型映射

## 关键寄存器

### 系统寄存器

- `ICC_BPR0_EL1`: Group 0优先级分组
- `ICC_BPR1_EL1`: Group 1优先级分组
- `ICC_CTLR_EL1`: GIC控制寄存器
- `ICC_EOIR0_EL1`: Group 0中断结束
- `ICC_EOIR1_EL1`: Group 1中断结束
- `ICC_IAR0_EL1`: Group 0中断确认
- `ICC_IAR1_EL1`: Group 1中断确认
- `ICC_PMR_EL1`: 中断优先级屏蔽
- `ICC_SGI0R_EL1`: Group 0 SGI生成
- `ICC_SGI1R_EL1`: Group 1 SGI生成

### ARE (Affinity Routing Enable)

ARE是GICv3关键特性：
- 启用亲和性路由
- 支持多级CPU亲和性（Affinity 0/1/2/3）
- 灵活的中断路由策略

## 安全状态

支持安全和非安全状态：
- Group 0中断：安全状态
- Group 1中断：非安全状态
- EL3可配置安全路由

## 相关概念

- [[GIC中断控制器]]
- [[ITS]]
- [[中断路由]]
- [[SGI]]
- [[SPI]]
- [[PPI]]

[^1]: 来源: raw/notes/ARM/gic/gic_v3.md