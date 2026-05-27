---
type: knowledge-node
tags: [core-concept, auto-generated, ARMv9, security]
last_compiled: 2026-05-22
---

# RME (Realm Management Extension)

RME 是 ARMv9 架构的安全扩展，实现机密计算环境[^1]。

## 四个物理地址空间 (PAS)

| PAS | 描述 |
|-----|------|
| Secure | 安全状态 |
| Non-secure | 非安全状态 |
| Root | 根世界 (Root world) |
| Realm | 领域世界 (Realm world) |

## 核心机制

- [[GPC]] (Granule Protection Check): 物理地址层面隔离[^1]
- [[GPT]] (Granule Protection Table): 定义每个物理页的 PAS归属[^1]

## Granule 生命周期

| 状态 | 描述 |
|------|------|
| UNDELEGATED | 未委托给 RMM |
| DELEGATED | 已委托但未使用 |
| RD | Realm Descriptor |
| REC | Realm Execution Context |
| DATA | Realm 数据 |

## SMMU 集成

SMMUv3 通过以下机制支持 RME[^1]：

- 对所有 client 访问执行 GPC
- 支持 SMMU_ROOT_GPT_BASE 寄存器

## 相关概念

- [[GPC]]
- [[SMMU]]
- [[机密计算]]

[^1]: 来源: raw/notes/ai_gen/ARMv9_RME_GPC_GPT_Notes.md