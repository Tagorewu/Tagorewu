---
title: 'FG、LG、PG(PE)的定义及区别'
date: Fri, 24 Jul 2015 07:03:09 +0000
draft: false
categories:
  - "Hardware"
tags: ['FG', 'Hardware Design']
---

FG與LG 都是接地端子 FG(Frame Ground): 是針對基板電路屏蔽用的接地端子, 主要是對高頻游離電磁波的屏蔽接地之用, 理論上應接於大地，一般将金属外壳，散热片，信号线缆屏蔽层等接于此，将噪声导入大地，或者防止触电。就是通常所认为的GR、PG(Protective Ground)，接地电阻要小于等于100欧姆。 LG(Line Ground)又叫(Functional Ground)不是(Logic Ground)： 是針對AC電源側低頻濾波器的接地, 主要是保持輸入電壓的電位準之用, 理論應與中性線連接。如果噪声是错误的主要来源 或者 有电击的问题， 则将FG 接入PG并一同接地，接地电阻要小于等于100欧姆。 所以 FG端是屬於信號端的接地, LG端是屬於電源端的接地, 二者雖都是接地, 在設計上端子仍是要分開的, 但一般在使用上, 除非AC中性線有帶電的現像, 否則你可以將FG及LG併接後一次接地既可。