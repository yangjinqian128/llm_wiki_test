---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, NUMA, migration]
last_compiled: 2026-05-22
---

# NUMA迁移校验

NUMA迁移校验是 QEMU [[热迁移]] 中确保源端和目标端 NUMA 拓扑一致性的机制[^1]。

## 问题背景

- ACPI 表 (SRAT/SLIT) 作为 guest RAM 迁移[^1]
- 目标端按自己的 `-numa` 参数构建内部 `numa_state`[^1]
- 两端不一致会导致 guest NUMA 调度策略错误[^1]

## 当前校验现状

| 检查项 | 状态 |
|--------|------|
| 内存总量 | 有校验 (RAMBlock 匹配) |
| CPU 数量 | 有校验 (VMState) |
| NUMA node 数量 | **无校验** |
| CPU-to-node 绑定 | **无校验** |
| NUMA 距离矩阵 | **无校验** |

## 核心风险

- 性能严重下降（跨 node 访问内存）[^1]
- 不会 crash，问题隐蔽[^1]

## 校验方案

在 machine 层注册 vmstate[^1]：

```c
typedef struct NumaMigrationState {
    uint16_t num_nodes;
    uint64_t node_mem[MAX_NODES];
    uint16_t node_cpus[MAX_NODES];
    uint8_t  distance[MAX_NODES][MAX_NODES];
} NumaMigrationState;
```

在 `post_load` 中校验[^1]：

- node 数量一致
- 每个 node 内存一致
- SLIT 距离矩阵一致

## 相关概念

- [[NUMA]]
- [[热迁移]]
- [[ACPI表结构解析]]

[^1]: 来源: raw/notes/ai_gen/QEMU_NUMA_Migration_Check.md