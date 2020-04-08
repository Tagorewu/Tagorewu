---
author: wuqt
date: 2018-08-22 03:12:29+00:00
draft: false
title: petalinux use guide summarize
type: post
url: /
categories:
- Linux
tags:
- debug kernel
- petalinux
- qemu
---

petalinux 是针对 Xilinx FPGA-based SOC designs 的 embedded linux system development kit 。
包含了:












 	  1. Yocto Extensible SDK
 	  2. Minimal downloads
 	  3. XSCT and tool chains
 	  4. PetaLinux CLI tools

详情参考 PetaLinux Tools Reference Guide。










* * *








**版本： v2018.2 June 6, 2018   **




不支持 ubuntu1804, 手贱装了ubuntu1804, 结果 build 的时候出现 Bitbake 运行不起来，google 一遍发现这版本petalinux  还是暂时不支持 ubuntu1804 ，遂重新装回1604.
总结：









 	  * 

 	    * petalinux 安装, 略。
 	    * BSP 安装

 	      * BSP 是petalinux 的一个参考开发板的基础配置。可以把它作为一个模板创建自己的工程。它提供 了一个 installable BSP files，包含了所有必要的 design 和 configuration files ，pre-built 和 tested hardwrae 和 software images.
 	      * 

    
    $ petalinux-create -t project -s <path-to-bsp>


为了快速走一遍 petalinx，使用BSP 作为 project source 。如果是自己设计了硬件平台，参考后续步骤。






 	  * 
 	  * 使用vivado 创建硬件平台。Export Hardware,  ( .hdf/.dsa ) file.
 	  * 创建 petalinux project.


 	  1. 

 	    1. 

 	      * 

    
    $ petalinux-create -type project --template <CPU_TYPE> --name <PROJECT_NAME>









 	  * 

 	    * 

 	      * <CPU_TYPE> 根据自己选择 zynqMP | zynq | microblaze






 	  * 

 	    * 
 	    * 导入硬件配置

 	      * 

    
     $ petalinux-config --get-hw-description=<PATH-TO_HDF/DSA_DIRECTORY>









 	  * 

 	    * 
 	    * petalinux-config -c kernel //配置内核
petalinux-config -c rootfs //配置文件系统
 	    * petalinux-build
 	    * petalinux-package --boot --fsbl ./images/linux/zynq_fsbl.elf --fpga --u-boot --force  //生成BOOT 文件
 	    * 最后将 BOOT.BIN 和image.ub 复制到SD卡上启动就行。
 	    * 在 QEMU 中调试 linux kernel

 	      * 

    
    $ petalinux-boot --qemu --kernel


，在运行参数里面可以看到 -gdb tcp:<TCP_PORT>
 	      * 再开一个 comand console,  就可以用gdb 连接上了

    
    $ cd "<plnx-proj-root>/images/linux"
    $ petalinux-util --gdb vmlinux
     (gdb) target remote :9000



 	      * 注:  kernel 配置要选上调试信息

    
    $ petalinux-config -c --kernel 
    > Kernel hacking > kernel debugging。














<blockquote></blockquote>
