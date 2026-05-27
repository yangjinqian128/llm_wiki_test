---
type: knowledge-node
tags: [core-concept, auto-generated, ARM64, MMU, page-table]
last_compiled: 2026-05-22
---

# BBML2 (Break-Before-Make Level 2)

BBML2 是 ARM 架构特性 FEAT_BBM 的 Level 2 实现，通过 `ID_AA64MMFR2_EL1.BBM` 字段报告[^1]。

## 架构定义

```
ID_AA64MMFR2_EL1.BBM 编码:
  0b0000 → BBM Level 0: 无 BBM 保证
  0b0001 → BBM Level 1: 部分 BBM 支持
  0b0010 → BBM Level 2: 完整 BBM 支持
```

## 核心语义

BBML2 允许软件在不执行中间 TLBI+DSB 的情况下做变更[^1]：

1. 改变描述符类型（block ↔ table）
2. 改变 block/table 的覆盖范围
3. 修改 PTE 的权限位（AP、PXN、XN）
4. 设置/清除 contiguous bit
5. 修改内存类型属性（部分场景）

## CPU vs SMMU BBML2

| 维度 | CPU FEAT_BBM L2 | SMMU IDR3.BBM L2 |
|------|-----------------|------------------|
| TLB conflict abort | 允许产生 | 禁止产生 |
| 检测方式 | MIDR 白名单 | 直接读 IDR3 |
| 一致性保证 | 二选一: abort 或一致结果 | 单一路径: 必须返回一致结果 |

## 内核 "noabort" 语义

内核通过 MIDR 白名单判断 CPU 是否支持 noabort[^1]：

```c
static const struct midr_range supports_bbml2_noabort_list[] = {
    MIDR_REV_RANGE(MIDR_CORTEX_X4, 0, 3, 0xf),
    MIDR_REV_RANGE(MIDR_NEOVERSE_V3, 0, 2, 0xf),
    ...
};
```

## 主要使用场景

- [[mTHP]] Contiguous PTE 折叠/展开[^1]
- 内核线性映射权限变更[^1]
- [[KFENCE]] Pool 初始化[^1]

## 相关概念

- [[SMMU]]
- [[SVA]]
- [[Stage2大页拆分]]
- [[页表]]

[^1]: 来源: raw/notes/ai_gen/ARM_CPU_BBML2_Analysis.md, raw/notes/ai_gen/ARM_SMMU_BBML2_Analysis.md