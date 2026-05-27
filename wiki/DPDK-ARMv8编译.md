---
title: DPDK ARMv8编译
tags: [dpdk, arm, compile, network]
created: 2026-05-22
source: raw/notes/net/compile_dpdk_in_armv8
---

# DPDK ARMv8编译

在ARMv8机器上编译DPDK[^1]。

## 环境准备

```bash
# 安装依赖
yum install kernel-headers.aarch64
yum install libpcap-devel.aarch64
yum install numactl-devel.aarch64
```

## 编译步骤

```bash
git clone git://dpdk.org/dpdk

make config T=arm64-armv8a-linuxapp-gcc
make
# 编译文件在 dpdk/build

# 或使用install方式
make install T=arm64-armv8a-linuxapp-gcc
```

## 编译示例

```bash
export RTE_SDK=~/repos/dpdk
export RTE_TARGET=arm64-armv8a-linuxapp-gcc
cd examples/helloworld/
make
```

## 运行测试

```bash
# 配置大页
echo 2 > /sys/kernel/mm/hugepages/hugepages-524288kB/nr_hugepages

# 运行
./helloworld -c 3 -n 2
```

## 相关概念

- [[DPDK大页内存]]
- [[DPDK内存池]]
- [[iperf网络测试]]

[^1]: raw/notes/net/compile_dpdk_in_armv8