---
title: 'zynq 嵌入式开发总结'
date: Sat, 20 Apr 2019 13:59:51 +0000
draft: false
tags: ['Embeded system', 'zynq linux;']
---

![](http://www.wuquantai.com/wp-content/uploads/2018/04/微信图片_20180413232224-1024x768.jpg)

### 传统开发步骤

**基础设计**

```
# 使用Vidado 设计硬件工程，导出硬件设计 bitstream.
$ File-> Export-> Export Hardware...
# 使用SDK 生成 fsbl.elf

# 编译U-boot 
$ make CROSS_COMPILE=arm-xilinx-linux-gnueabi-  zynq_ac7020_defconfig 

# 使用SDK， Xilinx Tools->Create Boot Image 生成BOOT.BIN。
  即:  fsbl.elf + system_wrapper.bit + u-boot.elf
```

**编译kernel**

```
$ make ARCH=arm CROSS_COMPILE=arm-xilinx-linux-gnueabi- uImage LOADADDR=0x00008000 
# 生成 /arch/arm/boot/uImage
```

**编译device tree**

```
$ dtc -I dts -O dtb devicetree.dtb ./arch/arm/boot/dts/AC7020.dts
```

**制作文件系统**

```
$ 基于RAM 的 ramdisk
$ 基于Flash 的 jffs2
$ 基于network 的 NFS
```

将 BOOT.bin, devicetree.dtb, uImage 拷贝至SD卡的FAT 分区。文件系统拷贝至ext 分区即可。

这是传统的嵌入式系统开发方式，后面 xilinx 出了petalinux 工具，降低了 uboot, kernel, filesystem 的系统配置工作量，可以从Vivado 导出的硬件信息自动完成相关软件的配置。