---
title: gcov
aliases: [gcov, 代码覆盖率, 测试覆盖率]
tags: [测试, 覆盖率, gcc]
source: raw/notes/gcov_note
---

# gcov

GCC 代码覆盖率测试工具。

## 原理

运行时建立跳转位置表，跳转前插入指令更新统计。

程序运行完生成数据文件，工具统计覆盖率。

## 使用方法

```bash
gcc test.c -c -fprofile-arcs -ftest-coverage -o main.o
gcc main.o -lgcov -o test
./test
gcov test.c
```

## 编译选项

| 选项 | 说明 |
|------|------|
| `-fprofile-arcs` | 记录分支执行次数 |
| `-ftest-coverage` | 记录代码行执行次数 |

## 输出文件

- `test.c.gcov`：覆盖率报告
- `test.gcda`：运行时数据
- `test.gcno`：编译时数据

## 反汇编对比

对比正常和 gcov 版本反汇编，可见插入的统计指令。

## 相关概念

- [[mtrace]] - 内存泄露检测
- [[内存分析工具]] - 内存调试
- [[Perf]] - 性能分析

---

**参考来源**：[gcov_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/gcov_note)