---
title: CPU Hotplug
aliases: [CPU热插拔, CPU上下线]
tags: [kernel, cpu, hotplug]
source: raw/notes/cpuhp_note
---

# CPU Hotplug

CPU在上下线时需要执行附带的配置动作，如PMU配置、功耗管理、指令功能配置等。

## 内核接口

内核通过`cpuhp_setup_state_xxx`系列函数提供回调注册接口：
- `cpuhp_setup_state_nocalls`
- `cpuhp_setup_state_multi`

## 注册模块

主要注册CPU上下线回调的模块：

| 层级 | 模块 |
|------|------|
| driver | perf, clocksource, irq, devfreq, cpuidle |
| kernel | trace, fork, softirq, crash_core |
| mm | page-writeback, zswap, page_alloc, slub |
| kvm | KVM相关 |
| arch/arm64 | cpuinfo, armv8_deprecated, cpufeature, fpsimd |

## 实现机制

内核使用大数组`struct cpuhp_step cpuhp_hp_states[]`保存所有CPU上下线回调，各厂家的回调函数统一注册到此数组中。

## 相关概念

- [[内核线程]] - CPU上下线时线程迁移
- [[cpuidle]] - CPU空闲状态管理
- [[PMU]] - 性能监控单元配置