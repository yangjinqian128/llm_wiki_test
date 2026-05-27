---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, virtualization]
last_compiled: 2026-05-22
---

# Postcopy迁移

Postcopy 是 [[热迁移]] 的一种模式，目的端 VM 先启动，缺页时从源端拉取[^1]。

## 与 Precopy 区别

| 模式 | VM 启动时机 | 数据传输 |
|------|------------|---------|
| Precopy | 迁移完成后 | 先传输全部数据 |
| Postcopy | 迁移过程中 | 缺页时拉取 |

## 源端流程

```
postcopy_start()
  ├─ 停止 VM
  ├─ qemu_savevm_state_postcopy_prepare()
  ├─ ram_postcopy_send_discard_bitmap()
  ├─ 发送 POSTCOPY_LISTEN → 目的端启动监听
  ├─ 发送打包设备状态
  └─ 发送 POSTCOPY_RUN → 目的端启动 VM
```

## 目的端流程

```
postcopy_listen_thread()
  ├─ state → POSTCOPY_DEVICE
  ├─ qemu_loadvm_state_main()
  │    ├─ 处理页面请求
  │    └─ 接收 REQ_PAGES 响应
  └─ state → POSTCOPY_ACTIVE → COMPLETED
```

## 缺页处理

```
目的端 VM 访问未迁移页面
  → userfaultfd 缺页
  → 发送 MIG_RP_MSG_REQ_PAGES 到源端
  → 源端 ram_save_page_request() 处理
  → 源端发送页面
  → 目的端 UFFDIO_COPY 写入
  → 唤醒 vCPU
```

## 恢复机制

网络中断时可恢复[^1]：

```
POSTCOPY_ACTIVE → POSTCOPY_PAUSED
  → POSTCOPY_RECOVER_SETUP
  → POSTCOPY_RECOVER
  → POSTCOPY_ACTIVE
```

## 相关概念

- [[热迁移]]
- [[脏页跟踪]]
- [[userfaultfd]]

[^1]: 来源: raw/notes/ai_gen/QEMU_Live_Migration_Flow.md