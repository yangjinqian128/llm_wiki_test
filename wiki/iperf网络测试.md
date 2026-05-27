---
title: iperf网络测试
created: 2026-05-22
tags:
  - 网络
  - 性能测试
  - 工具
---

# iperf网络测试

## 概述

iperf是网络性能测试工具，用于测量TCP/UDP带宽性能。

## 服务端

```bash
iperf -s -i 1
```

- `-s`：服务端模式
- `-i 1`：每秒打印一次结果

## 客户端

```bash
iperf -c <ip> -i 1
```

- `-c <ip>`：连接服务端IP
- `-i 1`：打印间隔

## 常用参数

| 参数 | 说明 |
|------|------|
| `-t` | 测试时长（秒） |
| `-P` | 并行流数量 |
| `-u` | UDP测试 |
| `-b` | 目标带宽（UDP） |
| `-w` | TCP窗口大小 |

## 参见

- [[TCP协议]]
- [[网络性能调优]]

---

**参考来源**：[iperf_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/net/iperf_note)