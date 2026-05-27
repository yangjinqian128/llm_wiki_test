---
type: knowledge-node
tags: [core-concept, auto-generated, performance, pmu]
last_compiled: 2026-05-22
---

# Perf架构

[[Perf]]子系统依赖硬件PMU完成性能数据采集[^1]。

## PMU使用模式

| 模式 | 说明 |
|------|------|
| 计数模式 | 记录事件发生次数[^1] |
| 采样模式 | 计数到门限触发中断[^1] |

## Perf职责

1. 用户接口（命令解析、数据展示）[^1]
2. PMU抽象（支持不同硬件）[^1]
3. 进程控制（调度时启停统计）[^1]
4. 用户态工具（tools/perf）[^1]

## 用户态工具

```c
main → run_argv → handle_internal_command
    → run_builtin → cmd_stat
        → run_perf_stat → __run_perf_stat
            → evlist__prepare_workload  // fork子进程
            → create_perf_stat_counter   // perf_event_open
            → enable_counters            // ioctl enable
            → evlist__start_workload     // 启动子进程
```
[^1]

## perf_event_open

```c
int perf_event_open(struct perf_event_attr *attr, 
                    pid_t pid, int cpu, int group_fd,
                    unsigned long flags);
```

- `pid`/`cpu`：跟踪进程和CPU[^1]
- `attr.type/attr.config`：定义event[^1]
- `PERF_TYPE_RAW`：厂商自定义event[^1]

## 内核架构

### 系统调用入口

```c
perf_event_open
  → perf_install_in_context  // 注册到task_struct
```
[^1]

### 调度时同步

```c
__perf_event_task_sched_in
  → perf_event_context_sched_in
      → ctx_sched_in
          → event_sched_in
              → pmu->add(event)  // 恢复counter
```
[^1]

```c
__perf_event_task_sched_out
  → pmu->del(event)  // 停止counter并保存值
```
[^1]

### PMU驱动

```c
armv8_pmu_device_probe
  → armpmu_alloc  // 创建struct pmu
  → armpmu_register  // 注册回调
```
[^1]

PMU中断处理函数记录溢出时的程序上下文[^1]。

## TopDown模型

Intel提出的系统性能分析方法[^1]。

ARM64暂不支持直接使用perf得到TopDown结果[^1]。

## ARM扩展

- [[SPE]]：Statistical Profiling Extension[^1]
- Uncore PMU[^1]

## 相关概念

- [[Perf火焰图]] - 热点可视化
- [[Perf trace]] - Tracepoint跟踪
- [[SMMU-PMU]] - SMMU性能监控
- [[ftrace]] - 内核跟踪
- [[eBPF]] - 动态分析

[^1]: 来源：raw/notes/perf/Linux_perf构架分析