---
title: 'Beremiz 简介'
date: Thu, 10 Dec 2015 07:36:06 +0000
draft: false
tags: ['Beremiz', '未分类']
---

Beremiz 是一各为自动化技术提供开放性源代码的软件，它的PLC平台可以支持IEC61131-3 五种开发语言，Beremiz 提供的子项目：

1.  PLCOpen 编辑器 : 提供自动化技术所需要的多平台 IDE （Integrated Development Environment） ；
2.  MatPLC's IEC 编译器 : IEC 61131-3 编译器， 可以将结构化文本和指令表120 直接编译为 C 语言；
3.  CanFestival : CANOpen 接口服务于物理的输入/ 输出；
4.  SVGUI : 以 SVG为基础的自动化 HMI 工具； 所有软件都是提供源代码的， 使用了 PYTHON MINGW 等语言或者平台。

Beremiz软PLC 开发流程: MatIEC包含两个工具: 1. IEC2IEC工具将标准的IEC61131-3的5种语言统一转成ST格式， 2. IEC2C工具将ST格式用户代码转ANSI-C代码。 然后将所有代码通过GCC 工具编译成机器码运行。 ![](http://www.beremiz.org/iec.png)