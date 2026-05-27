---
title: DPDK大页内存
created: 2026-05-22
tags:
  - DPDK
  - 大页内存
  - 网络
  - 性能优化
---

# DPDK大页内存

## 概述

DPDK(Data Plane Development Kit)使用大页内存(Huge Pages)提升数据包处理性能。

## 大页内存优势

- 减少TLB miss
- 减少页表遍历开销
- 降低内存管理开销

## DPDK实现

核心代码路径：`lib/librte_eal/linux/eal_hugepage_info.c`

关键函数：`hugepage_info_init`

## 配置方法

```bash
# 配置大页数量
echo 1024 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages

# 挂载hugetlbfs
mount -t hugetlbfs nodev /dev/hugepages
```

## 参见

- [[大页内存]]
- [[内存管理]]

---

**参考来源**：[dpdk_huge](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/net/dpdk_huge)