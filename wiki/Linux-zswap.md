---
title: Linux zswap
tags: [linux, memory, compression, swap]
created: 2026-05-22
source: raw/notes/crypto/zswap_note
---

# Linux zswap

zswap是Linux内核压缩swap内存的特性，先压缩内存再写入swap设备[^1]。

## 使用方法

### 内核配置

```
CONFIG_ZSWAP
CONFIG_ZBUD  // 压缩内存分配器
```

### 启动参数

```
zswap.enable=1
zswap.compressor=xxx  // 默认LZO
```

### 控制参数

/sys/module/zswap/parameters/:
- `compressor`: 选择压缩算法
- `enabled`: 使能功能
- `max_pool_percent`: 压缩池占内存百分比
- `zpool`: 后端分配器(zbud)
- `same_filled_pages_enable`: 相同页优化[^1]

### 创建swap分区

```bash
mkswap /dev/sda
swapon /dev/sda
swapon -s  # 查看swap分区
```

## 测试方法

使用cgroup构造内存压力场景：

```bash
cgcreate -g memory:bob
echo 0x4000000 > /sys/fs/cgroup/memory/bob/memory.limit_in_bytes
cgexec -g memory:bob memhog 128m
```

查看 `/sys/kernel/debug/zswap/stored_pages`[^1]。

## 相关概念

- [[内存管理]]
- [[内存回收]]
- [[crypto acomp架构]]

[^1]: raw/notes/crypto/zswap_note