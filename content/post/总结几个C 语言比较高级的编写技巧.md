---
title: '总结几个C 语言比较高级的编写技巧'
date: Fri, 25 Nov 2016 08:51:40 +0000
draft: false
categories:
  - "Software"
tags: ['C 编程']
---

看了一些源代码，这里总结几个不常见的，但是比较好用的C 用法。也许这些功能在C++中有扩展，但是个人基本不用C++。其实这些应该是属于预处理器的功能。若是之前没有见过，遇到时会感觉云里雾里的。

* * *

* 结构体直接赋值

  C99 支持直接将一个结构常量赋值给对应的结构体变量。而不用每个结构成员逐一赋值。

  ```c
  typedef struct rectangle{
      int x0;
      int y0;
      int x1;
      int y1;
  }rect_t;
  
  void main()
  {
      rect_t a = {0, 0, 0, 0};
      a = (rect_t) {0, 0, 1, 2};
  }
  
  ```

*   如何在编译阶段就知道结构体长度？

    可以定义一个数组指针：

    ```c
    char (*__kaboom)[sizeof( YourTypeHere )] = 1;
    ```

    编译时它就会产生一个warning，这样就可以知道它的长度了:

    ```c
    warning C4047: 'initializing': 'DWORD (*)[88]' differs in levels of indirection from 'int'
    ```

    同样也可以用 offsetof() 就知道成员的偏移量了。

* **直接定义字符串：**

  我们可以用#define 定义字符串常量，比如： #define HELLO   "hello" 这样就相当于是： char \*HELLO= "hello"; 而且使用#define 来定义的好处是可以直接拼接字符串，这些应该都是由预处理器来帮我们处理的（没有仔细考证，但想像其中原理应该是这样）。比如：```
  #define HELLO "hello"
  #define WORLD "world"

  puts( HELLO WORLD );

* **定义内存池：**

  在嵌入式系统中一般不用内存堆的动态分配，因为容易产生内存碎片导致大块内存分配不到（可以用带小块内存合并功能内存管理方式），或者是会因为某个模块把内存堆耗尽了导致其它模块不能工作。通常会使用内存池的方式，而且每个软件模块有自己的不同大小固定数量内存池，这样可以做到每个模块内存互不干扰。说白一点就是一系列的分配好空间的数组。这可以参考lwip中内存池的分配。 

```c
#define LWIP\_MEMPOOL(name,num,size,desc) u8\_t memp\_memory\_ ## name ## \_base \[((num) \* (MEMP\_SIZE + MEMP\_ALIGN\_SIZE(size)))\];
```

### 

