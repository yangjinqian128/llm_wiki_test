---
type: knowledge-node
tags: [core-concept, auto-generated, ARMv9, RME, security]
last_compiled: 2026-05-22
---

# GPC (Granule Protection Check)

GPC 是 ARMv9 [[RME]] 架构的核心硬件机制，在物理地址层面实现四个物理地址空间之间的内存隔离[^1]。

## 四个 PAS (Physical Address Space)

| PAS | 描述 |
|-----|------|
| Secure | 安全状态 |
| Non-secure | 非安全状态 |
| Root | 根世界 |
| Realm | 领域世界 |

## GPT 表结构

两级页表结构[^1]：

```
Level 0 GPT
    └── 指向 Level 1 GPT 描述符 (可覆盖 1GB ~ 512GB)

Level 1 GPT
    └── 包含 16 个 GPI 字段，每个描述一个物理页
```

## GPI (Granule Protection Information)

| GPI 值 | 含义 |
|--------|------|
| 0x0 | NO_ACCESS - 禁止所有访问 |
| 0x8 | SECURE |
| 0x9 | NON_SECURE |
| 0xA | ROOT |
| 0xB | REALM |
| 0xF | ANY - 允许所有访问 |

## GPC 检查流程

```
1. VA → Stage-1/Stage-2 translation → PA
2. 检查 PA 是否超出 GPC 范围
3. GPT walk → GPI
4. 比较 Access PAS vs GPI
5. 匹配 → 允许访问
   不匹配 → GPF (Granule Protection Fault)
```

## 关键寄存器

| 寄存器 | 描述 |
|--------|------|
| GPTBR_EL3 | Granule Protection Table Base Register |
| GPCCR_EL3 | GPT Configuration Control Register |

## 相关概念

- [[RME]]
- [[SMMU]]
- [[页表]]
- [[内存管理]]

[^1]: 来源: raw/notes/ai_gen/ARMv9_RME_GPC_GPT_Notes.md