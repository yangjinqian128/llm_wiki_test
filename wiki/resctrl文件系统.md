---
title: resctrl文件系统
tags: [Linux, 资源控制, MPAM, RDT]
created: 2026-05-22
source: raw/notes/ARM/MPAM基本逻辑分析.md
---

# resctrl文件系统

Linux资源控制文件系统接口，用于cache、内存带宽等硬件资源的分区和监控。Intel RDT和ARM MPAM均使用此接口。

## 目录结构

```
/sys/fs/resctrl/
├── cpus                     # CPU列表
├── cpus_list                # 人类可读CPU列表
├── mon_groups/              # 监控组目录
├── info/                    # 系统资源信息
│   ├── L3/
│   │   ├── cbm_mask
│   │   ├── min_cbm_bits
│   │   ├── num_closids
│   │   └── shareable_bits
│   ├── MB/
│   │   ├── bandwidth_gran
│   │   ├── delay_linear
│   │   └── min_bandwidth
│   └── last_cmd_status
├── mon_data/                # 根控制组监控数据
│   ├── mon_L3_00/
│   │   ├── llc_occupancy
│   │   ├── mbm_total_bytes
│   │   └── mbm_local_bytes
├── schemata                 # 资源分配方案
├── size                     # 根控制组缓存大小
├── tasks                    # 进程列表
└── <用户创建的目录>/        # 用户控制组
    ├── cpus
    ├── schemata
    ├── mon_groups/
    └── mon_data/
```

## 使用方式

resctrl使用层次化结构控制资源：
1. 根目录是全局资源
2. 用户创建目录形成控制组和监控组
3. 用户创建的目录和partition ID对应
4. 通过配置`cpus/cpus_list/tasks`实现partition ID和CPU/线程绑定

## 监控组

控制组创建后自动生成`mon_groups`和`mon_data`目录：
- `mon_groups`: 监控组控制目录
- `mon_data`: 监控组数据目录

需手动在mon_group里创建自定义监控组。

## schemata文件

定义资源分配方案，格式如：
```
L3:0=7f;1=7f
MB:0=20;1=30
```

## 内核实现

- Intel: `fs/resctrl/` + `arch/x86/kernel/cpu/resctrl/`
- ARM MPAM: `drivers/platform/mpam/`

核心数据结构：
- `struct mpam_class`: MPAM设备分类
- `struct mpam_msc`: MSC设备表示
- `struct mpam_resctrl_res`: 与resctrl交互的数据结构

## 相关概念

- [[MPAM]]
- [[MSC]]
- [[Partition ID]]

[^1]: 来源: raw/notes/ARM/MPAM基本逻辑分析.md