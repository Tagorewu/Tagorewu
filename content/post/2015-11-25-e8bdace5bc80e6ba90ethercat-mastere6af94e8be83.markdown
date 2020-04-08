---
author: wuqt
date: 2015-11-25 02:52:26+00:00
draft: false
title: '[转]开源EtherCAT Master比较'
type: post
url: /
categories:
- MotionControl
tags:
- ethercatmaster
- SOEM
---

EtherCAT的主站开发是基于EtherCAT机器人控制系统的开发中非常重要的环节。目前常见开源的主站代码为的[RT-LAB](http://www.rt-labs.com/)开发的[SOEM](http://ethercat.rt-labs.com/ethercat) (Simple OpenSource EtherCAT Master)和[EtherLab](http://www.etherlab.org/)的[the IgH EtherCAT® Master](http://www.etherlab.org/)。使用起来SOEM的简单一些，而the IgH EtherCAT® Master更复杂一些，但对EtherCAT的实现更为完整。

具体比较如下表：
<table cellpadding="0" width="649" style="height: 1384px;" cellspacing="0" border="0" >
<tbody >
<tr >

<td width="236" valign="top" >功能
</td>

<td width="331" valign="top" >SOME(Simple OpenSource EtherCAT Master)
</td>

<td width="454" valign="top" >IgH EtherCAT Master
</td>
</tr>
<tr >

<td width="236" valign="top" >版本
</td>

<td width="331" valign="top" >1.3.0
</td>

<td width="454" valign="top" >1.5.2
</td>
</tr>
<tr >

<td width="236" valign="top" >更新日期
</td>

<td width="331" valign="top" >2013-02-26
</td>

<td width="454" valign="top" >2013-02-12
</td>
</tr>
<tr >

<td width="236" valign="top" >发布公司
</td>

<td width="331" valign="top" >RT-LAB
</td>

<td width="454" valign="top" >EtherLab
</td>
</tr>
<tr >

<td width="236" valign="top" >官方网站
</td>

<td width="331" valign="top" >ethercat.rt-labs.com
</td>

<td width="454" valign="top" >[www.etherlab.org](http://www.etherlab.org/)
</td>
</tr>
<tr >

<td width="236" valign="top" >支持的操作系统
</td>

<td width="331" valign="top" >Linux,Windows
</td>

<td width="454" valign="top" >Linux
</td>
</tr>
<tr >

<td width="236" valign="top" >支持RT内核
</td>

<td width="331" valign="top" >RTAI, Xenomai
</td>

<td width="454" valign="top" >RTAI, Xenomai, RT-Preempt
</td>
</tr>
<tr >

<td width="236" valign="top" >支持的CPU
</td>

<td width="331" valign="top" >Freescale i.MX53

Blackfin 5xx

Blackfin 6xx

Intel
</td>

<td width="454" valign="top" >支持Linux内核的所有CPU
</td>
</tr>
<tr >

<td width="236" valign="top" >支持的网卡
</td>

<td width="331" valign="top" >-
</td>

<td width="454" valign="top" >8139too - RealTek 8139C (or compatible) Fast-Ethernet chipsets.

•e1000 - Intel PRO/1000 Gigabit-Ethernet chipsets (PCI).

•e100 - Intel PRO/100 Fast-Ethernet chipsets.

•r8169 - RealTek 8169/8168/8101 Gigabit-Ethernet chipsets.

•e1000e - Intel PRO/1000 Gigabit-Ethernet chipsets (PCI Express).
</td>
</tr>
<tr >

<td width="236" valign="top" >CANOpen over EtherCAT (CoE)
</td>

<td width="331" valign="top" >√
</td>

<td width="454" valign="top" >√
</td>
</tr>
<tr >

<td width="236" valign="top" >Vendor over EtherCAT (VoE)
</td>

<td width="331" valign="top" >√
</td>

<td width="454" valign="top" >√
</td>
</tr>
<tr >

<td width="236" valign="top" >Distributed clocks
</td>

<td width="331" valign="top" >√
</td>

<td width="454" valign="top" >-
</td>
</tr>
<tr >

<td width="236" valign="top" >SERCOS over EtherCAT (SoE)
</td>

<td width="331" valign="top" >√
</td>

<td width="454" valign="top" >√
</td>
</tr>
<tr >

<td width="236" valign="top" >Ethernet over EtherCAT (EoE)
</td>

<td width="331" valign="top" >×
</td>

<td width="454" valign="top" >√
</td>
</tr>
<tr >

<td width="236" valign="top" >File Access over EtherCAT (FoE)
</td>

<td width="331" valign="top" >×
</td>

<td width="454" valign="top" >√
</td>
</tr>
<tr >

<td width="236" valign="top" >Safety over EtherCAT (FSoE)
</td>

<td width="331" valign="top" >×
</td>

<td width="454" valign="top" >×
</td>
</tr>
</tbody>
</table>
