---
title: cache line
aliases: [缓存行]
tags: [hardware, cache, alignment]
---

# cache line

Cache的基本存储单元，通常为32-128字节。

## 对齐

数据对齐到cache line边界可提高访问效率，避免跨行访问。

## False Sharing

多核修改同一cache line的不同数据导致频繁cache一致性操作。

参见[[Cache一致性]]。

## 相关概念

- [[Cache]] - cache结构
- [[Cache一致性]] - 一致性协议
- [[并发编程]] - 对齐优化