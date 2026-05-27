---
type: knowledge-node
tags: [core-concept, auto-generated, iommu, performance]
last_compiled: 2026-05-22
---

# SMMU PMU

[[SMMU PMU]]（PMCG）提供SMMU性能统计[^1]。

## 驱动加载

内核模块：`arm_smmuv3_pmu`（`CONFIG_ARM_SMMU_V3_PMU`）[^1]。

需UEFI中有PMCG节点[^1]。

## 检测加载

```bash
dmesg | grep pmcg
# arm-smmu-v3-pmcg arm-smmu-v3-pmcg.8.auto: Registered PMU @ 0x0000000148020000 
#   using 8 counters with Individual filter settings
```
[^1]

## 事件类型

```bash
perf list | grep pmcg
# smmuv3_pmcg_140020/tlb_miss/
# smmuv3_pmcg_140020/config_cache_miss/
# smmuv3_pmcg_140020/transaction/
# smmuv3_pmcg_140020/cycles/
```
[^1]

## PMCG命名

格式：`smmuv3_pmcg_<phys_addr_page>`（基地址去掉低12bit）[^1]。

## 统计特定程序

```bash
perf stat -e smmuv3_pmcg_<phys_addr>/tlb_miss/ <your_program>
```
[^1]

## 按设备过滤

```bash
perf stat -e smmuv3_pmcg_<phys_addr>/tlb_miss/,
    filter_enable=1,filter_span=0,filter_stream_id=0x75 <your_program>
```

`filter_stream_id`即设备的BDF[^1]。

## 自定义事件

```bash
perf stat -e smmuv3_pmcg_<phys_addr>/event=0x80/ <your_program>
```
[^1]

## 相关概念

- [[SMMU]] - 硬件架构
- [[TLBI]] - TLB操作
- [[Perf架构]] - 性能分析框架

[^1]: 来源：raw/notes/iommu/smmu_pmu