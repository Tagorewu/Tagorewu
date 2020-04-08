---
author: wuqt
date: 2015-12-12 15:02:24+00:00
draft: false
title: Eclipse ARM IDE 开发环境搭建
type: post
url: /
categories:
- Embeded system
tags:
- ARM
- Eclipse
- emIDE
- Yagarto
---

一、Eclipse

Eclipse的本身只是一个框架平台，但是众多插件的支持，使得Eclipse拥有较好的灵活性。依托于Java 环境运行，所以必须安装 Jre。

二、CDT

CDT是Eclipse用于扩展Eclipse支持C/C++开发的插件。可直接下载带CDT的Eclipse。

三、Zylin CDT

支持Eclipse用于嵌入式C/C++开发和远程调试的插件。

四、Yagarto

Yagarto是整合了GNU arm的交叉编译工具链，是一个跨平台的 ARM 架构开发平台。他们说了，由于基于MinGW的ToolChain 的GDB 跟Eclipse 配合不是很好，所以Yagarto 出现了。目前Yagarto 项目已经完结。此外Yagrato 建议使用免费的



	  * [emIDE](http://www.emide.org/) (free Visual Studio Style IDE including GNU Tools for ARM) 。其集成了GNU Tools。

五、J-LINK GDB Server

A JTAG GDB Debug agent run on Host

六、IDE整体结构框图

![](http://img.my.csdn.net/uploads/201212/13/1355400422_7977.jpg)




安装：

1、安装Java SE

下载地址：[http://www.oracle.com/technetwork/java/javase/downloads/jre-7u3-download-1501631.html](http://www.oracle.com/technetwork/java/javase/downloads/jre-7u3-download-1501631.html)

设置环境变量:

如果只安装Jre的话就添加 : JAVA_HOME = C:\Program Files\Java\jre1.8.0_65;

若是安装JDK的话就添加 : JAVA_HOME = C:\Program Files\Java\jdk1.8.0_65;

Path = _%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;_

//安装JDK时，JDK内部有个jre目录，外部也默认安装了一个jre目录。一般配置jre环境为内部jre目录。

CLASSPATH = .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;

这里CLASSPATH包含了三个目录：

.; //.表示用户当前目录，别丢了。

%JAVA_HOME%\lib\dt.jar;

%JAVA_HOME%\lib\tools.jar;

2、下载eclipse

下载地址：[http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/)

可直接下载 Eclipse IDE for C/C++ Developers版本。

下载地址：[http://www.eclipse.org/cdt/downloads.php](http://www.eclipse.org/cdt/downloads.php)

4、安装Zylin Embedded CDT

可在Eclipse 里 Help - Install software里安装, 地址为: http://opensource.zylin.com/zylincdt 。

下载地址：[http://opensource.zylin.com/embeddedcdt.html](http://opensource.zylin.com/embeddedcdt.html)

5、安装Eclipse下开发ARM的插件

下载地址：[http://sourceforge.net/projects/gnuarmeclipse/files/](http://sourceforge.net/projects/gnuarmeclipse/files/)

下载后解压，把plugins/org.eclipse.cdt.cross.arm.gnu_0.5.3.201007311800.jar文件放入Eclipse安装目录下的plugins目录里

6、安装 arm-none-eabi-gcc 编译器

下载地址：https://launchpad.net/gcc-arm-embedded

![download icon](https://launchpad.net/@@/download)
** [gcc-arm-none-eabi-5_2-2015q4-20151219-win32.exe](https://launchpad.net/gcc-arm-embedded/5.0/5-2015-q4-major/+download/gcc-arm-none-eabi-5_2-2015q4-20151219-win32.exe)**

GNU Tools for ARM Embeded Processors. 包含了一系列的GNU 工具。


