---
title: BIO
aliases: [Block I/O]
tags: [block, io, storage]
---

# BIO

块I/O请求的基本数据结构。

## 结构

描述一次块设备读写操作，包含扇区地址、数据缓冲、大小等信息。

## 流程

参见[[块设备子系统]]，BIO → Request → request_queue。

## 相关概念

- [[块设备子系统]] - BIO流程
- [[blk-mq]] - 多队列处理
- [[blkring]] - 高性能队列实现