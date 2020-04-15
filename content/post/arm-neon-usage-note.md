---
title: 'ARM NEON Usage Note'
date: Fri, 07 Dec 2018 08:02:10 +0000
draft: false
categories:
  - "Embedded"
tags: ['ARM', 'ARM', 'NEON']
---

![](/images/uploads/2018/04/微信图片_20180413232200-1024x768.jpg)

**简介**
------

**SIMD**, 即Single Instruction Multiple Data（单指令多数据）的并行操作。CPU 在处理向量数据时有它的局限性。CPU的优势在于处理复杂多变的指令，而对于那种大数据量的重复性操作，ARM 为了增加处理效率，增加了这种并行处理模块, 即NEON(Advanced SIMD)。主要是在ARMv7 架构后的处理器使用。

NEON 的主要 components：

*   NEON register file
*   NEON integer execute pipeline
*   NEON single-precision floating-point execute pipeline
*   NEON load/store and permute pipeline

NEON 指令和 floating-point 指令使用的是相同的 register file。不同于ARM core的register file。此 register file 可以以 32-bit, 64-bit, 128-bit 方式访问。 The contents of the NEON registers are vectors of elements of the same data type. A vector is divided into lanes and each lane contains a data value called an element. 通常NEON 并行操作数量n, 就等于vectors 拆分的lanes（通道）数。例如：

64-bit NEON vectors can contain:

*   —Eight 8-bit elements.
*   —Four 16-bit elements.
*   —Two 32-bit elements.
*   —One 64-bit element.

128-bit NEON vectors can contain:

*   —Sixteen 8-bit elements.
*   —Eight 16-bit elements.
*   —Four 32-bit elements.
*   —Two 64-bit elements.

NEON 单元总共的 register file 资源可以视作:

*   16个 128-bit的 Q(quadword) 寄存器，Q0-Q15。
*   32个 64-bit 的 D(Doubleword)寄存器，D0~D31。

NEON 指令通常有几种使用方式：

*   使用预先用NEON 优化好的库, 主要是一些Mechine Learning 和 computer Vision的库。如: Ne10，libyuv, skia 等。
*   AutoVectorization （ 自动向量化编译器）
*   NEON intrinsics
*   NEON assembly

这里主要说一下 intrinsics  方式。

* * *

intrinsics
----------

相较于 assambly模式， intrinsic 更易于使用和记忆

*   **vector datatypeatatype**

```
<type><size>x<number\_of\_lanes>\_t
```

For example:

*   int16x4\_t is a vector describes a vector of four 16-bit short int value

*   float32x4\_t describes a vector of four 32-bit float values

也可以组成多个vector的数组

```
<type><size>x<number\_of\_lanes>x<length\_of\_array>\_t
``````
 struct int16x4x2\_t{          
    int16x4\_t val\[2\];  
 };
```

*   **Prototype of NEON intrinsics**

```
<opname><flags>\_<type>
```

An additional q flag is provided to specify that the intrinsic operates on 128-bit vectors. For example:

*   vmul\_s16, multiplies two vectors of signed 16-bit values. This compiles to VMUL.I16 d2, d0, d1.
*   vaddl\_u8, is a long add of two 64-bit vectors containing unsigned 8-bit values, resulting in a 128-bit vector of unsigned 16-bit values. This compiles to VADDL.U8 q1, d0, d1.

*   **使用 NEON intrinsics**

Intrinsics that use the ‘q’ suffix usually operate on Q registers. Intrinsics without the ‘q’ suffix usually operate on D registers but some of these intrinsics might use Q registers.  
The examples below show different variants of the same intrinsic.

```
uint8x8\_t vadd\_u8(uint8x8\_t a, uint8x8\_t b);
```

The intrinsic vadd\_u8 does not have the ‘q’ suffix. In this case, the input and output vectors are 64-bit vectors, which use D registers.

```
uint8x16\_t vaddq\_u8(uint8x16\_t a, uint8x16\_t b);
```

The intrinsic vaddq\_u8 has the ‘q’ suffix, so the input and output vectors are 128-bit vectors, which use Q registers.

```
uint16x8\_t vaddl\_u8(uint8x8\_t a, uint8x8\_t b);
```

The intrinsic vaddl\_u8 does not have the ‘q’ suffix. In this case, the input vectors are 64-bit and output vector is 128-bit.

```
#include <arm\_neon.h>
uint32x4\_t double\_elements(uint32x4\_t input){
    return(vaddq\_u32(input, input));
}
```

* * *

参考资料：  
[ARM NEON](https://developer.arm.com/technologies/neon)  
[A\_neon\_programmers\_guide\_en.pdf](https%3A%2F%2Fstatic.docs.arm.com%2Fden0018%2Fa%2FDEN0018A_neon_programmers_guide_en.pdf)