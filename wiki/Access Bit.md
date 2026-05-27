---
title: Access Bit
tags: [CPU, 页表, MMU]
created: 2026-05-22
source: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md
---

# Access Bit

页表项中的访问位，CPU访问页面时由硬件置位，用于内存管理。

## 定义

页表项中access bit:
- CPU访问（读或写）页面时置为1
- 置位动作由硬件完成
- 软件可读取判断页面访问情况

## 功能

用途：
- 页面交换（swap）：判断页面是否活跃
- 内存回收：优先回收未访问页面
- 内存管理策略：基于访问频率

## 硬件逻辑

CPU访问页面时：
1. MMU做地址翻译
2. 硬件置位页表项access bit
3. 若TLB有access bit信息，也更新TLB

## TLB同步

TLB是页表项的cache：
- TLB需记录access bit
- 硬件更新access bit时同步更新TLB
- TLB和页表项需保持一致

同步方式：
- **软件同步**: 软件更新页表项后flush TLB
- **硬件同步**: 硬件自动同步TLB和页表项

不同同步方式对应不同软硬件接口定义。

## 与Dirty Bit区别

Access bit:
- 访问（读或写）时置位
- 表示页面被访问

Dirty bit:
- 仅写时置位
- 表示页面被修改
- 页面换出时需写回磁盘

## 相关概念

- [[Dirty Bit]]
- [[页表]]
- [[TLB]]
- [[页面交换]]

[^1]: 来源: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md