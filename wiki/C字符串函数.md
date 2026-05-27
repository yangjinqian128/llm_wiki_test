---
title: C字符串函数
tags: [c, string, glibc]
created: 2026-05-22
source: raw/notes/C_C++/c_string_func
---

# C字符串函数

glibc字符串处理函数[^1]。

## strcmp/strncmp

字符串比较[^1]。

## strcpy/strncpy

字符串复制[^1]。

## strcat/strncat

字符串连接[^1]。

## strrchr/strchr

```c
char *strrchr(const char *s, int c);  // 从右找
char *strchr(const char *s, int c);   // 从左找

char *file_type(char *f) {
    char *cp;
    if ((cp = strrchr(f, '.')) != NULL)
        return cp + 1;  // 返回扩展名
    return "";
}
```

## strstr

查找子串[^1]。

## isalnum/isxxx

字符类型判断[^1]。

## 相关概念

- [[C文件IO函数]]
- [[C内存函数]]
- [[C语言宏]]

[^1]: raw/notes/C_C++/c_string_func