---
type: knowledge-node
tags: [core-concept, auto-generated, SMMU, SVA]
last_compiled: 2026-05-22
---

# SVA依赖BBML2

[[SVA]] (Shared Virtual Addressing) 在 [[SMMU]]v3 上安全工作硬性依赖 [[BBML2]][^1]。

## 核心问题

SVA 场景下 CPU 和 SMMU 共享同一个进程页表[^1]：

- CPU MM 子系统管理页表
- SMMU 页表遍历器异步并发遍历

若 CPU 修改页表时 SMMU 正在遍历[^1]：

```
CPU 修改 PTE → [cache line 传播中]
SMMU TLB walker 读取该 PTE
  → 可能看到: 旧值、新值、或中间态/撕裂值
  → BBM0: 行为 UNPREDICTABLE
  → 可能产生 TLB conflict abort (不可恢复)
```

## 为什么软件无法解决

1. CPU 修改页表时不会插入 SMMU TLBI[^1]
2. 无法"暂停" SMMU 遍历[^1]
3. 修改点高度分散[^1]

## BBML2 的保证

将 SMMU 行为从 UNPREDICTABLE 收紧为[^1]：

- 要么重试遍历
- 要么返回一致的旧值或新值
- **明确禁止产生 TLB conflict abort**

## 代码验证

```c
bool arm_smmu_sva_supported(struct arm_smmu_device *smmu)
{
    u32 feat_mask = ARM_SMMU_FEAT_COHERENCY;
    if (system_supports_bbml2_noabort())
        feat_mask |= ARM_SMMU_FEAT_BBML2;
    if ((smmu->features & feat_mask) != feat_mask)
        return false;
    ...
}
```

## 相关概念

- [[SVA]]
- [[SMMU]]
- [[BBML2]]

[^1]: 来源: raw/notes/ai_gen/ARM_SMMU_BBML2_Analysis.md