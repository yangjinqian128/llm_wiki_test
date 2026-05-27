---
title: RoCE测试
tags: [network, roce, rdma, test]
created: 2026-05-22
source: raw/notes/net/how_to_test_roce
---

# RoCE测试

RoCE驱动的sanity测试方法[^1]。

## 环境准备

```bash
# 下载工具
git clone https://github.com/lsgunth/perftest.git
git clone https://github.com/linux-rdma/rdma-core.git

# 安装依赖(RedHat)
yum install rdma-core-devel.aarch64
yum install libnl-devel.aarch64
```

## 编译和安装

```bash
# 编译perftest
./configure && make

# 编译rdma-core
cmake . && make

# 安装用户态库
cp rdma-core/build/lib/* /lib64/

# 内核和模块
modprobe hns-roce-hw-v2
```

## 测试步骤

1. 查看设备: `/sys/class/infiniband`[^1]
2. up网络接口，保持link状态[^1]
3. 配置driver: `echo "driver hns" > /etc/libibverbs.d/hns.driver`[^1]

## 性能测试

```bash
# 服务端
./ib_write_bw -n 5 -d hns_0 &

# 客户端
./ib_write_bw -n 5 -d hns_0 192.168.2.198

# 其他测试
./ib_read_bw
./ib_send_bw
```

## 相关概念

- [[HNS3驱动架构]]
- [[iperf网络测试]]
- [[设备驱动]]

[^1]: raw/notes/net/how_to_test_roce