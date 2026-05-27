---
title: DPDK内存池
created: 2026-05-22
tags:
  - DPDK
  - 内存池
  - 无锁队列
  - 性能优化
---

# DPDK内存池

## 概述

DPDK提供rte_mempool内存池，用于高效管理固定大小的内存块。

## 架构

```
               +------------+
     cpu0      |local_cache |----+
               +------------+    |
                                 |
               +------------+    +-> +---------+           +-------------+
     cpu1      |local_cache |------> |rte_ring |---------->|memory block |
               +------------+    +-> +---------+           +-------------+
                                 |
               +------------+    |
     cpuN      |local_cache |----+
               +------------+
```

### 数据结构

| 结构 | 说明 |
|------|------|
| memzone | 分配的内存（大页内存） |
| rte_ring | 无锁队列管理内存块 |
| local_cache | 每CPU的缓存 |

## 性能优化

- **rte_ring**：使用CAS原子指令实现无锁队列
- **local_cache**：减少多核竞争，CPU优先从本地cache获取
- 批量操作：cache不足时批量从ring获取

## 核心接口

| 接口 | 说明 |
|------|------|
| rte_mempool_create | 创建内存池 |
| rte_mempool_cache_create | 创建缓存 |
| rte_mempool_generic_get | 申请内存块 |
| rte_mempool_generic_put | 释放内存块 |

## 实现路径

```
rte_mempool_create
  +-> rte_mempool_set_ops_name      /* 设置ops */
  +-> rte_mempool_populate_default  /* 申请内存 */
    +-> mempool_ops_alloc_once
      +-> ops->alloc                 /* ring初始化 */

rte_mempool_generic_get
  +-> get from local cache
  +-> rte_mempool_ops_dequeue_bulk  /* 从ring获取 */
```

参见[[DPDK大页内存]]、[[无锁队列]]。

---

**参考来源**：[dpdk_mempool_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/net/dpdk_mempool_note)