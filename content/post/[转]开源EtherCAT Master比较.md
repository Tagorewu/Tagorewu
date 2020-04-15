---
title: '[转]开源EtherCAT Master比较'
date: Wed, 25 Nov 2015 02:52:26 +0000
draft: false
categories:
  - "Automation"
tags: ['ethercatmaster', 'MotionControl', 'SOEM']
---

EtherCAT的主站开发是基于EtherCAT机器人控制系统的开发中非常重要的环节。目前常见开源的主站代码为的[RT-LAB](http://www.rt-labs.com/)开发的[SOEM](http://ethercat.rt-labs.com/ethercat) (Simple OpenSource EtherCAT Master)和[EtherLab](http://www.etherlab.org/)的[the IgH EtherCAT® Master](http://www.etherlab.org/)。使用起来SOEM的简单一些，而the IgH EtherCAT® Master更复杂一些，但对EtherCAT的实现更为完整。 具体比较如下表：

功能

SOME(Simple OpenSource EtherCAT Master)

IgH EtherCAT Master

版本

1.3.0

1.5.2

更新日期

2013-02-26

2013-02-12

发布公司

RT-LAB

EtherLab

官方网站

ethercat.rt-labs.com

[www.etherlab.org](http://www.etherlab.org/)

支持的操作系统

Linux,Windows

Linux

支持RT内核

RTAI, Xenomai

RTAI, Xenomai, RT-Preempt

支持的CPU

Freescale i.MX53 Blackfin 5xx Blackfin 6xx Intel

支持Linux内核的所有CPU

支持的网卡

\-

8139too - RealTek 8139C (or compatible) Fast-Ethernet chipsets. •e1000 - Intel PRO/1000 Gigabit-Ethernet chipsets (PCI). •e100 - Intel PRO/100 Fast-Ethernet chipsets. •r8169 - RealTek 8169/8168/8101 Gigabit-Ethernet chipsets. •e1000e - Intel PRO/1000 Gigabit-Ethernet chipsets (PCI Express).

CANOpen over EtherCAT (CoE)

√

√

Vendor over EtherCAT (VoE)

√

√

Distributed clocks

√

\-

SERCOS over EtherCAT (SoE)

√

√

Ethernet over EtherCAT (EoE)

×

√

File Access over EtherCAT (FoE)

×

√

Safety over EtherCAT (FSoE)

×

×