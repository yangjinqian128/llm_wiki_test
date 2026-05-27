---
type: knowledge-node
tags: [core-concept, auto-generated, kernel-tracing]
last_compiled: 2026-05-22
---

# ftrace

[[ftrace]] 是 Linux 内核自带的函数追踪工具，用于跟踪内核代码执行流程和函数执行时间[^1]。

## 配置要求

使用前需确保内核打开编译选项 `CONFIG_FTRACE`[^1]。

## 使用方法

基本追踪流程：
```bash
cd /sys/kernel/debug/tracing
echo 0 > tracing_on           # 关闭trace
echo 0 > trace                # 清空buffer
echo function_graph > current_tracer  # 设置跟踪器
echo funcgraph-proc > trace_options
echo funcgraph-abstime > trace_options  # 添加时间戳
echo your_func > set_ftrace_filter     # 设置目标函数
echo 1 > tracing_on            # 开始trace
```
[^1]

## 输出分析

输出包含 `TIME`、`CPU`、`TASK/PID`、`DURATION` 和 `FUNCTION CALLS` 列[^1]。

`DURATION` 列显示每个函数的执行时间，通过时间戳差值可计算函数间时延[^1]。

## 调用链追踪

使用 `set_graph_function` 可以追踪函数的完整调用链[^1]。

[^1]: 来源：raw/notes/ftrace_func