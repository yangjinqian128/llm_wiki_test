---
title: C网络函数
tags: [c, network, socket, glibc]
created: 2026-05-22
source: raw/notes/C_C++/c_net_func
---

# C网络函数

glibc网络相关函数[^1]。

## 主机相关

- `gethostname`: 获取主机名[^1]
- `gethostbyname`: 解析主机名[^1]

## Socket相关

- `socket`: 创建socket[^1]
- `bind`: 绑定地址[^1]
- `listen`: 监听[^1]
- `accept`: 接受连接[^1]
- `connect`: 连接[^1]

## 数据传输

- `recvfrom`: 接收数据[^1]
- `sendto`: 发送数据[^1]

## 地址转换

- `inet_ntoa`: IP地址转字符串[^1]
- `ntohs`: 网络字节序转主机字节序[^1]

## 相关概念

- [[C文件IO函数]]
- [[RoCE测试]]
- [[iperf网络测试]]

[^1]: raw/notes/C_C++/c_net_func