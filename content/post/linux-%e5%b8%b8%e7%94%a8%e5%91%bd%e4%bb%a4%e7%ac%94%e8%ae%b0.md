---
title: 'linux 常用命令笔记'
date: Fri, 20 Nov 2015 06:33:12 +0000
draft: false
tags: ['Linux', 'tar']
---

*   1\. 修改文件夹所有者

chown -R \[owner\] ..          ; 修改当前目录下所有文件夹的所有都为owner, -R表示递归调用。

*   2\. 解压命令

tar -zxvf xxx;

\-c: 建立压缩档案 -x：解压 -t：查看内容 -r：向压缩归档文件末尾追加文件 -u：更新原压缩包中的文件 这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。 -z：有gzip属性的 -j：有bz2属性的 -Z：有compress属性的 -v：显示所有过程 -O：将文件解开到标准输出 下面的参数-f是必须的 -f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

**创建用户** useradd **修改密码** passwd **安装包** apt-get install apt-get update //更新源 **iptables** iptables -nv -L //列出 **netstat** **ps aux** 查看进程