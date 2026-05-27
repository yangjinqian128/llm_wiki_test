---
type: knowledge-node
tags: [core-concept, auto-generated, iommu, fault]
last_compiled: 2026-05-22
---

# IOPF

[[IOPF]]（I/O Page Fault）处理设备DMA访问时的缺页[^1]。

## 处理流程

```
SMMU Event Queue → iommu_report_device_fault
                        ↓
                   添加fault list
                   设置timer expire
                        ↓
                   report to workqueue
                        ↓
                   handle_fault → iommu_page_response
                        ↓
                   remove from fault list
                   if fault list empty → del_timer
```
[^1]

## Fault类型

| type值 | 说明 |
|--------|------|
| IOMMU_FAULT_DMA_UNRECOV | 不可恢复错误[^2] |
| IOMMU_FAULT_PRM | 页请求消息[^2] |

## Tracepoint

内核定义`dev_fault` tracepoint[^2]：

```c
TRACE_EVENT(dev_fault,
    TP_PROTO(struct device *dev, struct iommu_fault *evt),
    TP_STRUCT__entry(
        __string(device, dev_name(dev))
        __field(int, type)
        __field(int, reason)
        __field(u64, addr)
        __field(u32, pasid)
        ...
    ),
)
```
[^2]

## Perf跟踪

```bash
sudo perf trace -e iommu:* test_program
```
[^2]

输出示例：
```
iommu:dev_fault:IOMMU:0000:76:00.0 type=2 reason=0 
addr=0x000000002321c000 pasid=1 group=138 flags=3 prot=1
```
[^2]

## Stalled Transaction

进程退出时，通过CD配置终止stalled transaction[^3]：

```c
exit_mmap → mmu_notifier_release
    → io_mm_release
        → arm_smmu_mm_clear
            → arm_smmu_write_ctx_desc
                → CD.S = CD.R = 0; EPD0 = 1;
                → arm_smmu_sync_cd (CD CFGI)
```
[^3]

## 相关概念

- [[SMMU]] - Event Queue
- [[PASID]] - 子流ID
- [[PRI]] - PCIe页请求
- [[SVA]] - 共享虚拟地址
- [[Perf trace]] - 缺页跟踪

[^1]: 来源：raw/notes/iommu/iopf_timer
[^2]: 来源：raw/notes/perf/perf_trace
[^3]: 来源：raw/notes/iommu/smmu_dev_stall_problem