---
title: 'linux bash shell 像cmd.exe 一样的滚动补齐功能'
date: Sun, 19 Nov 2017 14:40:14 +0000
draft: false
tags: ['Linux', 'shell completion']
---

有些情况下，比如我们遇到文件名含有一些字符不容易输入时就很着急了，这时候就需要让linux shell像windows cmd.exe 一样自动补全部的文件名，且可自动按顺序一个个滚动切换。方法如下： 在用户目录创建 ~/.inputrc (若不存在则自己创建) 文件里添加一行： \[code\]TAB: menu-complete\[/code\] 。 登出再登陆用户就可以了。 For more details see the `READLINE section` in `man bash` .