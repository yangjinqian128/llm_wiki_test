---
title: IPI
aliases: [Inter-Processor Interrupt, 核间中断]
tags: [中断, SMP, Linux内核]
---

# IPI

IPI(Inter-Processor Interrupt)是 CPU 核间相互发送中断的机制。

## ARM 实现

ARM 使用 SGI(Software Generated Interrupt)支持 IPI，详见 [[GIC中断控制器]]。

## Linux 接口

```c
smp_call_function(func, info, wait)
smp_call_function_many(mask, func, info, wait)
```

在指定 CPU 执行函数，`wait=true` 同步等待完成。

## IPI 类型

ARM64 IPI 中断号：
- `IPI_RESCHEDULE`: 重调度
- `IPI_CALL_FUNC`: 执行函数
- `IPI_CPU_STOP`: 停止 CPU
- `IPI_TIMER`: 定时器迁移
- `IPI_IRQ_WORK`: IRQ work

## 注册流程

```
gic_smp_init
  -> set_smp_ipi_range
    -> request_percpu_irq(IPI_*, ipi_handler)
```

## 发送流程

```
send_call_function_single_ipi
  -> arch_send_call_function_single_ipi
    -> smp_cross_call(cpu, IPI_CALL_FUNC)
      -> __ipi_send_mask
        -> gic_ipi_send_mask
          -> gic_write_sgi1r(ICC_SGI1R)
```

## 应用示例

唤醒进程：
```
try_to_wake_up
  -> __smp_call_single_queue
    -> send_call_function_single_ipi
```

## 处理流程

```
ipi_handler
  -> do_handle_IPI(ipinr)
    case IPI_RESCHEDULE:
      scheduler_ipi()
    case IPI_CALL_FUNC:
      generic_smp_call_function_interrupt()
```

## 性能测试

[ipi_benchmark](https://lkml.org/lkml/2017/12/19/141) 测试 IPI 延时。

## 相关概念

- [[软中断]]: IPI 触发软中断处理
- [[负载均衡]]: noHz 均衡使用 IPI
- [[GIC中断控制器]]: 硬件实现

[^1]: 来源: raw/notes/linux/linux_IPI