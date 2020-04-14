---
title: 'gdb 简记'
date: Wed, 21 Dec 2016 09:41:23 +0000
draft: false
tags: ['Embeded system', 'gdb']
---

常用命令如下：（详情可以用 help 命令查看）

* * *

r (run, restart) s (step into) n (next state, step over) finish (step out) q (quit) p (print anything you want) l (list) b (breakpoint) #设置断点： (gdb) b fileName.c:lineNumber 或者 (gdb) b function u (until) #运行到指定行 call  #调用函数 set  #设置变量 set args #设置main 的输入参数 bt (backtrace)  #查看堆栈 x #examine memory, (gdb)/FMT ADDRESS info # show things, (gdb) info break d # delete somthing (gdb) delete break xx 或者 (gdb) clean lineNumber 如果print 的内容比较长导致后面的内容是...，可以重新设置print长度的上限，或者无限制： set print element 0 如果要生成带调试信息的程序，在gcc时要加上-g参数。 target remote 192.168.1.220:1234 #连接远程 gdbserver show sysroot info sharedlibrary #显示共享库设置情况 set sysroot remtoe: #设置根目录到远程主机，连接远程共享库 set solib-search-path: #设置动态库搜索路径 dump binary memory \[dump.bin\] \[start\] \[end\] #dump 内存 gdb python #调试python 程序 (gdb) run xxx.py # 设置断点可以直接跳入 c库