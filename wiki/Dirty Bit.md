---
title: Dirty Bit
tags: [CPU, 页表, MMU]
created: 2026-05-22
source: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md
---

# Dirty Bit

页表项中的脏位，CPU写页面时由硬件置位，表示页面内容被修改。

## 定义

页表项中dirty bit:
- CPU写（修改）页面时置为1
- 置位动作由硬件完成
- 软件可读取判断页面是否被修改

## 功能

用途：
- 页面交换（swap）：脏页面换出时需写回磁盘
- 内存回收：脏页面回收成本高
- Copy-on-write：判断是否需复制页面

## 硬件逻辑

CPU写页面时：
1. MMU做地址翻译
2. 硬件置位页表项dirty bit
3. 若TLB有dirty bit信息，也更新TLB

## TLB同步

TLB需记录dirty bit：
- TLB entry包含dirty bit信息
- 硬件更新dirty bit时同步更新TLB
- TLB和页表项需保持一致

优化：
- 若TLB里dirty bit已是1
- 硬件可跳过更新页表项（已一致）
- 减少页表项写操作

同步方式：
- **软件同步**: 软件更新页表项后flush TLB
- **硬件同步**: 硬件自动同步

## 页面换出

脏页面换出流程：
1. 检查dirty bit
2. 若dirty bit=1，写回磁盘
3. 若dirty bit=0，直接丢弃（磁盘已有副本）
4. 更新页表项，释放物理页面

## 与Access Bit区别

Access bit:
- 读或写时置位
- 表示页面被访问

Dirty bit:
- 仅写时置位
- 表示页面被修改
- 换出时需写回

## 相关概念

- [[Access Bit]]
- [[页表]]
- [[TLB]]
- [[页面交换]]
- [[Copy-on-write]]

[^1]: 来源: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md