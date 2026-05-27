---
title: writeback
aliases: [write back, page writeback, dirty page writeback]
tags: [文件系统, 内存管理, Linux内核]
---

# writeback

writeback 是 Linux 将 page cache 脏页写回持久存储的机制。

## 基本流程

1. write 系统调用写入 page cache
2. 标记页面为脏
3. writeback 线程异步写回存储

## 数据结构

- `super_block`: 文件系统实例
- `backing_dev_info(bdi)`: 回写设备
- `bdi_writeback(wb)`: 回写相关数据
- `wb_writeback_work`: 回写任务

## write 系统调用

```
write -> vfs_write -> ext4_file_write_iter
  -> ext4_buffered_write_iter
    -> generic_perform_write
      -> ext4_write_end
        -> __mark_inode_dirty
      -> balance_dirty_pages_ratelimited
```

脏页超过比例时同步等待回写完成。

## writeback 线程

内核线程 `writeback` 在 `bdi_wq` workqueue 运行：
```
wb_workfn
  -> wb_writeback
    -> writeback_sb_inodes
      -> __writeback_single_inode
        -> do_writepages
          -> ext4_writepages
            -> submit_bio
```

## 强制回写

- `sync()`: 全系统脏页
- `syncfs(fd)`: 指定文件系统
- `fsync(fd)`: 指定文件

## 配置接口

`/proc/sys/vm/`
- `dirty_writeback_centisecs`: 周期回写间隔(默认 5s)
- `dirty_background_ratio`: 触发阈值(默认 10%)
- `dirty_ratio`: 最大阈值

## 触发机制

1. 定时触发: delay workqueue
2. 脏页超过阈值
3. 用户调用 sync

## tracepoint

- `writeback:global_dirty_state`: 脏页统计
- `block:block_rq_issue`: bio 下发

## 相关概念

- [[内存回收]]: 回收脏页需先写回
- [[块设备子系统]]: submit_bio 进入
- [[page cache]]: writeback 操作对象

[^1]: 来源: raw/notes/linux/Linux内核writeback逻辑分析