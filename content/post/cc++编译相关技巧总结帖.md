---
title: 'c/c++编译相关技巧总结帖'
date: Wed, 04 Apr 2018 07:35:04 +0000
draft: false
categories:
  - "Software"
tags: ['c 链接', 'Linux']
---

1.  链接时忽略文件中未用到的函数或者对象。这在移植代码过程中很有用, 我们就不需要去删除或者注释掉那些大量没用到的对象或者不需要去链接的对象。

For GCC, this is accomplished in two stages: First compile the data but tell the compiler to separate the code into separate sections within the translation unit. This will be done for functions, classes, and external variables by using the following two compiler flags:```
-fdata-sections -ffunction-sections
```Link the translation units together using the linker optimization flag (this causes the linker to discard unreferenced sections):```
-Wl,--gc-sections
```So if you had one file called test.cpp that had two functions declared in it, but one of them was unused, you could omit the unused one with the following command to gcc(g++):```
gcc -Os -fdata-sections -ffunction-sections test.cpp -o test -Wl,--gc-sections
```(Note that -Os is an additional compiler flag that tells GCC to optimize for size)