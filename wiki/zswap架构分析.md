---
title: zswap架构分析
tags: [linux, memory, zswap, compression]
created: 2026-05-22
source: raw/notes/crypto/zswap_arch
---

# zswap架构分析

zswap软件架构分析，为添加crypto acomp支持做准备[^1]。

## 初始化流程

1. 为每个cpu core分配per cpu dst内存[^1]
2. 为每个cpu core分配内核压缩上下文[^1]
3. 创建zpool，注册evict回调[^1]
4. 注册frontswap回调函数[^1]

## 核心数据结构

```c
struct zswap_pool {
    // per cpu压缩上下文
    // zpool实例
};

frontswap ops:
    .store    // 存入zpool
    .load     // 从zpool加载
    .invalidate_page  // 释放压缩页
    .invalidate_area
```

## store流程

```c
zswap_frontswap_store
  +-> zswap_entry_cache_alloc()     // 分配entry描述swap页
  +-> crypto_comp_compress()        // 调用crypto comp API压缩
  +-> zpool_malloc()                // 从zpool分配内存
  +-> memcpy()                      // 存入压缩后的页
  +-> zswap_rb_insert()             // 插入红黑树索引
```

## 关键问题

- 当前使用crypto comp接口，未使用acomp[^1]
- 压缩时关闭抢占，与acomp不兼容[^1]
- 压缩完需要copy到zpool[^1]

## 相关概念

- [[Linux-zswap]]
- [[crypto acomp架构]]
- [[内存管理]]

[^1]: raw/notes/crypto/zswap_arch