---
title: 'IAR sprintf() 输出不正常的问题'
date: Mon, 15 Aug 2016 00:57:44 +0000
draft: false
categories:
  - "Embedded"
tags: ['IAR sprintf 不正常', 'MCU']
---

前几天调试了一程序，用到sprintf函数，输出结果一直不正常，同样的输出格式用printf打印则没有问题。然后同样的程序在gcc下sprintf也是正常的。怀疑IAR的sprintf函数有问题。后来发现原因是IAR把sprintf函数简化了，在project-options-general options 里面的Library options 的Tabs里有个printf formatter，默认是选成tiny的选项，结果就不支持一些复杂的输出格式。把它设置成auto，输出结果就正常了，o yeah!