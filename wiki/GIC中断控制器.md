---
title: GIC中断控制器
created: 2026-05-22
tags:
  - ARM
  - 中断
  - GIC
  - 硬件
---

# GIC中断控制器

## 概述

GIC(Generic Interrupt Controller)是ARM架构的通用中断控制器，负责管理和分发中断到各CPU核心。

## GIC版本

- **GICv3**：支持系统寄存器接口、虚拟化
- **GICv4**：支持直接注入虚拟中断
- **GICv5**：最新版本

## 中断类型

### 按来源分类

- **SGI**(Software Generated Interrupt)：软件中断，用于核间通信
- **PPI**(Private Peripheral Interrupt)：私有外设中断，主要给时钟使用
- **SPI**(Shared Peripheral Interrupt)：共享外设中断，连接到distributor
- **LPI**(Locality-specific Peripheral Interrupt)：基于消息的中断

### 按状态分类

- **Inactive**：未激活
- **Pending**：等待处理
- **Active**：正在处理
- **Active and Pending**：正在处理且有新请求

## 架构组件

### Distributor

- 中断分发控制
- 中断优先级管理
- 中断路由

### Redistributor

- 连接到各个CPU core
- 处理SGI和PPI
- 支持虚拟化

### ITS(Interrupt Translation Service)

- 中断翻译服务
- 支持MSI/MSI-X
- 中断标识保持不变性

### CPU Interface

- 核一侧的系统寄存器接口
- **ICC_XXX_ELn**：物理中断控制寄存器
- **ICV_XXX_ELn**：虚拟中断控制寄存器
- **ICH_XXX_ELn**：虚拟中断控制

## 系统寄存器接口

GICv3支持系统寄存器访问方式（vs 内存映射方式）：

- 中断优先级管理
- 中断使能控制
- 中断确认和完成

## 虚拟化支持

Distributor、Redistributor和ITS都支持虚拟化，允许[[ARM系统寄存器|虚拟机]]直接管理中断。

## MBI中断

MBI(Message Based Interrupt)可以作为SPI上报，也可通过Redistributor接收MSI/MSI-X而无需ITS支持。

---

**参考来源**：[ARM_GIC硬件逻辑总结](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ARM/ARM_GIC硬件逻辑总结), [gic_v3.md](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ARM/gic/gic_v3.md)