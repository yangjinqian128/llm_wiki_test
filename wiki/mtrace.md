---
title: mtrace
aliases: [mtrace, 内存泄露检测, GNU mtrace]
tags: [调试, 内存, 内存泄露]
source: raw/notes/mtrace_note
---

# mtrace

GNU C 库内存泄露检测工具，检测 malloc/realloc/free 问题。

## 原理

1. `mtrace()` 为 malloc 等函数安装 handlers
2. malloc/free 执行时写信息到指定文件
3. Perl 脚本 `/usr/bin/mtrace` 解析信息

## 使用方法

```c
#include <mcheck.h>

int main(void) {
    mtrace();
    // ... malloc/free 操作
    return 0;
}
```

```bash
export MALLOC_TRACE="mtrace.out"
gcc -g test.c -o test
./test
mtrace test mtrace.out
```

## 输出示例

```
Memory not freed:
-----------------
Address     Size     Caller
0x1650490   0x28     at test.c:11
0x16504f0   0x28     at test.c:13
```

## 检测范围

| 函数 | 说明 |
|------|------|
| malloc | 内存分配 |
| realloc | 重新分配 |
| free | 内存释放 |

## 参考文档

- http://www.gnu.org/software/libc/manual/html_node/Allocation-Debugging.html
- http://www.gnu.org/software/libc/manual/html_node/Hooks-for-Malloc.html

## 相关概念

- [[内存分析工具]] - 其他内存调试工具
- [[GDB]] - 通用调试器
- [[Valgrind]] - 更强大的内存检测

---

**参考来源**：[mtrace_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/mtrace_note)