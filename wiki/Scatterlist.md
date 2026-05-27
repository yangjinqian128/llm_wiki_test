---
title: Scatterlist
aliases: [scatterlist, 聚散表, sg]
tags: [Linux内核, DMA, 内存管理]
source: raw/notes/scatterlist
---

# Scatterlist

Linux 内核聚散表数据结构，管理多段不连续物理内存。

## 为什么需要 Scatterlist

| 场景 | 内存模型 | 说明 |
|------|----------|------|
| CPU MMU | 连续虚拟 → 不连续物理 | 不需要 sg |
| 设备 DMA + IOMMU | 连续设备地址 → 不连续物理 | 不需要 sg |
| 设备 DMA 无 IOMMU | 支持聚散地址 | 需要 sg |

某些设备 DMA 支持配置多段离散物理地址，一次 DMA 访问全部。

## 使用流程

```
1. 分配内存区域（已知 CPU 虚拟地址）
2. 填充 scatterlist 结构
3. dma_map_sg 处理（得到 DMA 地址）
4. 配置设备 DMA 使用 DMA 地址
```

## API

```c
// 填充 sg
sg_set_buf(sg, cpu_addr, size);

// 映射 sg 得到 DMA 地址
dma_map_sg(dev, sg, nents, direction);

// 遍历 sg 获取 DMA 地址
for_each_sg(sg, s, nents, i) {
    dma_addr = sg_dma_address(s);
    dma_len = sg_dma_len(s);
}
```

## 数据结构

```c
struct scatterlist {
    unsigned long page_link;  // 页指针 + 标志位
    unsigned int offset;      // 页内偏移
    unsigned int length;      // 长度
    dma_addr_t dma_address;   // DMA 地址
};
```

## 参考文档

- https://lwn.net/Articles/234617/
- http://www.wowotech.net/memory_management/scatterlist.html

## 相关概念

- [[DMA]] - DMA 机制
- [[一致性DMA映射]] - DMA 映射类型
- [[IOMMU]] - 地址翻译
- [[dma-buf]] - 跨设备共享缓冲区

---

**参考来源**：[scatterlist](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/scatterlist)