---
title: 'keil 5.0： pack installer Reading one or more Pack discriptions failed 报错的问题'
date: Fri, 27 Dec 2013 13:31:52 +0000
draft: false
categories:
  - "Embedded"
tags: ['MCU']
---

http://www.keil.com/support/docs/3646.htm 按照链接所说的是Keil5.0不支持这些新版本的DFP，需要换Keil5.01. http://www.keil.com/dd2/pack/      有点奇怪这里有V1.0版本的DFP为什么导进去也不成功呢？ http://www2.keil.com/mdk5/legacy    后来下载了legacy support，包括Cortex M系列和ARM7和ARM9的在Keil4.73版本里面的两个Device 包，这个是Keil5.0支持的。 然后，果断升级了keil5.1. 发现了一个规律，只要在之前破解版的基础上覆盖安装到V5.01都是不需要再破解的直接就是标准版的了，再加上那两个包就能完全兼容老版本支持的设备。。