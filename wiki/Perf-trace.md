---
type: knowledge-node
tags: [core-concept, auto-generated, performance, trace]
last_compiled: 2026-05-22
---

# Perf trace

[[Perf trace]]跟踪内核tracepoint事件[^1]。

## Tracepoint定义

使用`TRACE_EVENT`宏定义[^1]：

```c
TRACE_EVENT(dev_fault,
    TP_PROTO(struct device *dev, struct iommu_fault *evt),
    TP_ARGS(dev, evt),
    TP_STRUCT__entry(
        __string(device, dev_name(dev))
        __field(int, type)
        __field(u64, addr)
        __field(u32, pasid)
        ...
    ),
    TP_fast_assign(...),
    TP_printk("IOMMU:%s type=%d addr=0x%llx pasid=%d", ...)
);
```
[^1]

## 插入Tracepoint

代码中调用`trace_dev_fault(dev, evt)`[^1]。

## 跟踪IO缺页

```bash
sudo perf trace -o log_sva -a -e iommu:* test_program
```
[^1]

注意：需要sudo权限和`-a`参数[^1]。

## 输出示例

```
iommu:dev_fault:IOMMU:0000:76:00.0 type=2 reason=0 
  addr=0x000000002321c000 pasid=1 group=138 flags=3 prot=1
```
[^1]

## Ftrace替代

也可通过sysfs使能tracepoint[^1]：

```bash
cd /sys/kernel/debug/tracing
echo 1 > events/iommu/dev_fault/enable
cat trace
```
[^1]

## 自定义打印

在代码中插入`trace_printk()`[^1]。

设置过滤：`echo ':mod:xxx_module_name' > set_ftrace_filter`[^1]。

## 相关概念

- [[Perf架构]] - Perf框架
- [[IOPF]] - IO缺页跟踪
- [[ftrace]] - 内核函数跟踪
- [[eBPF]] - 动态分析

[^1]: 来源：raw/notes/perf/perf_trace