---
title: 'recent new tech need to analysis and learn'
date: Fri, 01 Jun 2018 02:44:51 +0000
draft: false
categories:
  - "Embedded"
tags: ['Embeded system', '未分类']
---

**reverse ssh tunnel** nohup ssh -NfR 2222:localhost:22 root@106.12.13.xxx -p22 **expect script**   **strip command** Some useful line editing key bindings provided by the [Readline](http://cnswww.cns.cwru.edu/php/chet/readline/rltop.html) library:

*   Ctrl-A: go to the beginning of line
*   Ctrl-E: go to the end of line
*   Alt-B: skip one word backward
*   Alt-F: skip one word forward
*   Ctrl-U: delete to the beginning of line
*   Ctrl-K: delete to the end of line
*   Alt-D: delete to the end of word

[https://askubuntu.com/questions/45521/how-to-navigate-long-commands-faster](https://askubuntu.com/questions/45521/how-to-navigate-long-commands-faster)   **gprof tool** [https://blog.csdn.net/stanjiang2010/article/details/5655143](https://blog.csdn.net/stanjiang2010/article/details/5655143) [http://www.etalabs.net/compare\_libcs.html](http://www.etalabs.net/compare_libcs.html) [https://git.busybox.net/buildroot/tree/toolchain/uClibc/Glibc\_vs\_uClibc\_Differences.txt?id=6be5cb76b98091bc5623eb33776da94ce4715313](https://git.busybox.net/buildroot/tree/toolchain/uClibc/Glibc_vs_uClibc_Differences.txt?id=6be5cb76b98091bc5623eb33776da94ce4715313)```
uclibc 无法使用 gprof 进行profile。
可以使用以下工具：Profiling:
-------------------------------------------------------------------

uClibc no longer supports 'gcc -fprofile-arcs  -pg' style profiling, which
causes your application to generate a 'gmon.out' file that can then be analyzed
by 'gprof'.  Not only does this require explicit extra support in uClibc, it
requires that you rebuild everything with profiling support.  There is both a
size and performance penalty to profiling your applications this way, as well
as Heisenberg effects, where the act of measuring changes what is measured.

There exist a number of less invasive alternatives that do not require you to
specially instrument your application, and recompile and relink everything.

The OProfile system-wide profiler is an excellent alternative:
      http://oprofile.sourceforge.net/

Many people have had good results using the combination of Valgrind
to generate profiling information and KCachegrind for analysis:
      http://developer.kde.org/~sewardj/
      http://kcachegrind.sourceforge.net/

Prospect is another alternative based on OProfile:
      http://prospect.sourceforge.net/

And the Linux Trace Toolkit (LTT) is also a fine tool:
    http://www.opersys.com/LTT/

FunctionCheck:
	http://www710.univ-lyon1.fr/~yperret/fnccheck/
```

**neon** [https://community.arm.com/processors/b/blog/posts/coding-for-neon---part-1-load-and-stores](https://community.arm.com/processors/b/blog/posts/coding-for-neon---part-1-load-and-stores) **libc and ucLibc** [http://www.etalabs.net/compare\_libcs.html](http://www.etalabs.net/compare_libcs.html)   **libuv** \[hermit auto="1" loop="0" unexpand="1" fullheight="0"\]netease\_songs#:21303927\[/hermit\]