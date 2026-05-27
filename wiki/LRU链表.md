---
title: LRU链表
aliases: [LRU list, Least Recently Used]
tags: [内存管理, 页面回收, Linux内核]
---

# LRU链表

LRU 链表是 Linux 内核维护物理页面冷热信息的数据结构，用于指导内存回收。

## 结构

每个 NUMA 节点有 5 个 LRU 链表：
- active anonymous (活跃匿名页)
- inactive anonymous (非活跃匿名页)
- active file (活跃文件页)
- inactive file (非活跃文件页)
- unevictable (不可回收页)

## 状态转换

使用两位饱和计数器模式：
- `PG_referenced`: 访问标记
- `PG_active`: 活跃状态

转换逻辑类似硬件分支预测：访问触发状态变化，inactive 到 active，反之亦然。

## 关键流程

1. 页面分配时加入 inactive LRU
2. 页表项 access bit 置位表示访问
3. 回收流程检测 access bit，更新状态
4. 从 inactive LRU 回收物理页面

## 触发点

- `alloc_pages` 内存不足时
- `kswapd` 内核线程检测低于水线
- `damon` 主动检测

## 相关概念

- [[内存回收]]: 基于 LRU 链表回收页面
- [[DAMON]]: 主动管理冷热页
- [[伙伴系统]]: 回收后页面返回伙伴系统

[^1]: 来源: raw/notes/linux/mm/Linux内存管理-内存回收