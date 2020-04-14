---
title: '编译PYNQ 在ZCU102上的镜像'
date: Fri, 05 Apr 2019 12:46:54 +0000
draft: false
tags: ['AC7020', 'FPGA', 'PYNQ', 'ZCU102']
---

由于PYNQ官方没有编译好的ZCU102的镜像，所以需要自己手动编译。这里记录一下编译过程。  
因为手头上的ZCU102 批次比较新，所以目前只能使用2018.3 版本的SDK才能BOOT起来。（应该是由于换了DDR型号了，所以老版本的镜像是BOOT不起来的。板子版本是REVISION 1.1的 新批号）

#### PYNQ 版本Xilinx Tool 版本对应

Release version

Xilinx Tool Version

v1.4

2015.4

v2.0

2016.1

v2.1

2017.4

v2.2

2017.4

v2.3

2018.2

**v2.4**

**2018.3**

**参考：**  
[https://blog.csdn.net/vacajk/article/details/84728062](https://blog.csdn.net/vacajk/article/details/84728062)  
[](https://github.com/Xilinx/PYNQ/tree/image_v2.3/sdbuild)[https://github.com/Xilinx/PYNQ/tree/image\_v2.4/sdbuild](https://github.com/Xilinx/PYNQ/tree/image_v2.4/sdbuild)

[https://pynq.readthedocs.io/en/latest/pynq\_sd\_card.html#pynq-sd-card](https://pynq.readthedocs.io/en/latest/pynq_sd_card.html#pynq-sd-card)

#### 准备工作

*   使用事先编译好的文件系统: [bionic.aarch64.2.4.img](http://www.pynq.io/board.html)
*   ZCU102 BSP: [xilinx-zcu102-v2018.3-final.bsp](https://www.xilinx.com/member/forms/download/xef.html?filename=xilinx-zcu102-v2018.3-final.bsp)
    *   如果非官方板子，比如黑金的AC7020, 没有BSP, 则可以从vivado 工程导出hdf 文件，给petalinux 生成一个bsp
*   安装好SDSoc2018.3, PetaLinux2018.3
*   系统环境 Ubuntu 1604-64

#### 编译步骤

```
\# 下载 pynq
$ git clone https://github.com/Xilinx/PYNQ.git
$ cd PYNQ
$ git checkout image\_v2.4

# 检查依赖环境，qemu，crosstool-ng
$ cd ./sdbuild/
$ ./scripts/setup\_host.sh

# 准备 ZCU102 setting
$ cp -rf ./boards/ZCU104 ./boards/ZCU102
$ rm -rf ./boards/ZCU102/petalinux\_bsp/
$ mv ./boards/ZCU102/ZCU104.spec ./boards/ZCU102/ZCU102.spec

# 修改ZCU102.spec的内容， 先只保留以太网模块
$ vim ./boards/ZCU102/ZCU102.spec
> ARCH\_ZCU102 := aarch64
> BSP\_ZCU102 := xilinx-zcu102-v2018.3-final.bsp
> STAGE4\_PACKAGES\_ZCU102 := ethernet

# 拷贝 bsp 和 文件系统镜像
$ cp ~/xilinx-zcu102-v2018.3-final.bsp ./boards/ZCU102/
$ mkdir ./sdbuild/prebuilt
$ cp ~/bionic.aarch64.2.3.img ./sdbuild/prebuilt

# 根据自己安装路径配置 PATH
$ PATH=/opt/qemu/bin:/opt/crosstool-ng/bin:$PATH
$ source /opt/pkg/petalinux/settings.sh
$ source /opt/Xilinx/Vivado/2018.3/settings64.sh

# 编译，我的i7-7500U 编译BOOT大概15分钟，编译kernel image半个小时
$ sudo echo
$ make boot\_files BOARDS=ZCU102
$ sudo echo
$ make images BOARDS=ZCU102 PREBUILT=./prebuilt/bionic.aarch64.2.3.img

# done
$ ls ./output/
```

然后将imge 烧进SD卡启动，就可以愉快的玩耍PYNQ了！

若板子不提供bsp 文件, 以(AC7020) 为例，则准备boards文件夹如下：

```
$ cp -rf ./boards/PYNQ-Z2 ./board/AC7020
$ rm -rf ./boards/AC7020/petalinux\_bsp/
$ cp your.hdf ./boards/AC7020/petalinux\_bsp/hardware\_project/system.hdf
$ cp your.bit ./boards/AC7020/base/base.bit
$ vim ./boards/AC7020/AC7020.spec
ARCH\_AC7020 := arm
BSP\_AC7020 := 
BITSTREAM\_AC7020 := base/base.bit
STAGE4\_PACKAGES\_AC7020 := pynq ethernet

$ make boot\_files BOARDS=AC7020
# 即可有编译脚本调用petalinux 生成bsp

```