---
title: sbitmap
aliases: [Scalable Bitmap]
tags: [concurrent, bitmap, queue]
---

# sbitmap

可扩展位图，用于blk-mq等场景的高并发索引分配。

## 应用

参见[[块设备子系统]]，blk-mq使用sbitmap分配请求队列索引。

## 相关概念

- [[块设备子系统]] - sbitmap应用场景
- [[blk-mq]] - 多队列块层
- [[并发编程]] - 高并发数据结构