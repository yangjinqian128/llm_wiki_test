---
type: knowledge-node
tags: [core-concept, auto-generated, SMMU, ARM64]
last_compiled: 2026-05-22
---

# HTTU (Hardware Translation Table Update)

HTTU 是 ARM [[SMMU]]v3 的硬件特性，允许 SMMU 硬件自动更新页表中的 Access flag (AF) 和 Dirty flag[^1]。

## 支持级别

由 IDR0 寄存器 bits [7:6] 标识[^1]：

| IDR0.HTTU 值 | 含义 |
|---|---|
| 1 | 仅支持 Hardware Access flag (HA) |
| 2 | 同时支持 HA 和 Hardware Dirty (HD) |

## 工作机制

通过修改 LPAE 页表描述符的 bit[51] (DBM) 实现[^1]：

- **写时变脏**: 硬件对 writable PTE 写操作时，若 DBM=1，自动清除 AP[2] (AP_RDONLY)
- **判断脏状态**: `DBM=1 && AP_RDONLY=0` → 页面已脏

## 启用流程

```
探测 IDR0.HTTU → FEAT_HA / FEAT_HD
  ↓
Domain分配 IOMMU_HWPT_ALLOC_DIRTY_TRACKING → QUIRK_ARM_HD
  ↓
PTE构造 对 writable PTE 设置 DBM=1
  ↓
CD编程 设置 TCR_HA=1, TCR_HD=1
  ↓
运行时 硬件自动标记脏页
  ↓
脏页回收 遍历 PTE 检测 DBM=1 && AP_RDONLY=0
```

## 主要限制

- 仅支持 [[Stage-1]][^1]
- 不支持嵌套虚拟化[^1]
- [[SVA]] 路径未启用 HTTU[^1]
- 依赖硬件一致性[^1]

## 相关概念

- [[SMMU]]
- [[SVA]]
- [[脏页跟踪]]
- [[BBML2]]

[^1]: 来源: raw/notes/ai_gen/ARM_SMMU_HTTU_Analysis.md