---
title: C语言格式化打印
tags: [c, printf, format]
created: 2026-05-22
source: raw/notes/C_C++/c_note
---

# C语言格式化打印

printf格式化打印的特殊用法[^1]。

## %n格式符

%n返回已输出的字符数目，用于对齐[^1]。

```c
int num;
printf("123456:%n\n", &num);
printf("%*c %d\n", num, ' ', 123);
printf("%*c %d\n", num, ' ', 456);

// 输出:
// 123456:
//         123
//         456
```

## 相关概念

- [[C语言宏]]
- [[C字符串函数]]
- [[C文件IO函数]]

[^1]: raw/notes/C_C++/c_note