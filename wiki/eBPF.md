---
type: knowledge-node
tags: [core-concept, auto-generated, performance-analysis]
last_compiled: 2026-05-22
---

# eBPF

[[eBPF]]（extended Berkeley Packet Filter）允许用户态代码在内核中安全执行，用于性能分析和系统观测[^1]。

## 核心机制

[[eBPF]] 程序可以附着在内核已有的 [[tracepoint]]、[[kprobe]] 等探测点上[^1]。

每次内核执行到探测点时，都会运行用户定义的 [[eBPF]] 代码[^1]。

## 辅助函数与库

内核提供各种 helper 函数（如 `bpf_ktime_get_ns`、`bpf_log2l`）支持统计计算[^1]。

用户态库 [[bcc]] 简化了 [[eBPF]] 程序的编写和部署[^1]。

## 配置选项

需要开启的内核编译选项：
- `CONFIG_BPF_SYSCALL=y`
- `CONFIG_BPF_KPROBE_OVERRIDE=y`
- `CONFIG_KPROBE=y`
[^1]

## 应用场景

[[eBPF]] 可用于：
- 统计内核执行时间分布[^1]
- 分析 [[SVA]] 场景中的 IO 缺页次数与时延[^1]
- 生成 CPU 时间分布直方图[^1]

## 相关概念

- [[Perf架构]] - 性能采集框架
- [[Perf火焰图]] - 火焰图可视化
- [[Perf trace]] - Tracepoint跟踪
- [[ftrace]] - 内核函数跟踪

[^1]: 来源：raw/notes/ebpf_to_get_duration_dist