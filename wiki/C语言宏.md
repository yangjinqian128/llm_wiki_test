---
title: C语言宏
tags: [c, macro, preprocessor]
created: 2026-05-22
source: raw/notes/C_C++/c_note
---

# C语言宏

C语言宏的使用方法[^1]。

## 基本操作符

- `##`: 拼接前后符号[^1]
- `#`: 符号字符串化[^1]

## 展开规则

- 简单宏先展开内层再展开外层[^1]
- 使用##或#的宏直接应用规则[^1]
- 需要间接展开时使用中间宏[^1]

## 示例

```c
#define TEST(a, b) a##b
#define STR(a) #a
#define A 1000
#define _STR(a) STR(a)      // 中间宏实现间接展开

STR(TEST(1, 9))     // 输出 "TEST(1, 9)"
_STR(TEST(1, 9))    // 输出 "19"
```

## 用宏定义函数

```c
#define FUN(name, op)           \
int do_##name(int a, int b)     \
{                               \
    return a op b;              \
}

FUN(add, +)
FUN(sub, -)
```

## 注意事项

- 字符串里无法定义宏[^1]
- 参考: https://gcc.gnu.org/onlinedocs/cpp/

## 相关概念

- [[C语言格式化打印]]
- [[C文件IO函数]]
- [[C++ STL容器]]

[^1]: raw/notes/C_C++/c_note