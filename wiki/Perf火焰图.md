---
type: knowledge-node
tags: [core-concept, auto-generated, performance, visualization]
last_compiled: 2026-05-22
---

# Perf火焰图

[[火焰图]]可视化CPU热点分布[^1]。

## 生成步骤

1. 下载工具[^1]：
```bash
git clone https://github.com/brendangregg/FlameGraph
```

2. 采集数据[^1]：
```bash
perf record -a -g -F 100
```
- `-g`：记录调用栈
- `-F`：采样频率（16核系统每秒16×100采样）

3. 提取数据[^1]：
```bash
perf script > out.perf
```

4. 转换格式[^1]：
```bash
cp out.perf /path_to/FlameGraph/
/path_to/FlameGraph/stackcollapse-perf.pl out.perf > out.folded
```

5. 生成SVG[^1]：
```bash
/path_to/FlameGraph/flamegraph.pl out.folded > out.svg
```

6. 浏览器打开`out.svg`[^1]。

## 图形解读

- **宽度**：函数占用CPU时间比例[^1]
- **层次**：调用栈深度[^1]
- **颜色**：随机区分（无特殊含义）[^1]

## 相关概念

- [[Perf架构]] - 采集基础
- [[ftrace]] - 内核跟踪
- [[eBPF]] - 动态分析

[^1]: 来源：raw/notes/perf/perf_flame