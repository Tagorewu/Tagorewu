---
title: '嵌入式开学-先认识硬件'
date: Sun, 15 Dec 2013 15:28:03 +0000
draft: false
tags: ['Software Design']
---

这里使用的是TQ2440开发板，CPU 一般运行在400M的时钟频率。核心板载有两片Hynix(海力士,又叫现代内存)256Mb的SDRAM合64MB的内存，256MB SAMSUNG 的Nand flash，2MB 的Eon（宜扬） 的Nor flash。 首先，Nor flash 和 Nand flash 的区别？ Nor flash的有自己的地址线和数据线，操作以字为单位，可以采用类似于memory的随机访问方式，在nor flash上可以直接运行程序，所以nor flash可以直接用来做boot，采用nor flash启动的时候会把地址映射到0x00上。类似于计算机内BIOS的存放位置。 Nand flash是IO设备，类似于计算机的硬盘，数据、地址、控制线都是共用的，需要软件区控制读取时序，更**无法挂在ARM的程序空间，**所以不能像nor flash、内存一样随机访问，不能EIP（片上运行），因此不能直接作为boot。一般Nand flash 用来存储系统程序，系统上电后由Nor flash中的BootLoader将系统加载进RAM中的可执行地址中运行，然后跳到主程序中运行。（这有点像冯.诺伊曼架构，程序和数据都在RAM中）。Nand flash在读取数据量较大的成块数据时，速度较快。