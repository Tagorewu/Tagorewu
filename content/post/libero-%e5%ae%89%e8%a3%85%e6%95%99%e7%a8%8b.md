---
title: 'Libero 安装教程'
date: Thu, 07 Jan 2016 02:07:03 +0000
draft: false
tags: ['FPGA', 'libero 安装']
---

在此记录下各种软件的安装教程，等到重装电脑时就会发现它的好处了。 这里 Libero 的版本是11.5。V11.3的版本有bug，不建议装。 去官网申请一个免费的Gold 1 year 的license. 可使用 Vol C: 查看Disk 序列号。 安装软件完毕后。 1. 放置license.dat文件到C:\\flexlm\\ 下。 2. 如果license 不是用你的disk序列号申请的，可用硬盘序列号修改器修改C盘序列号 。 3. 添加系统环境变量。 LM\_LICENSE\_FILE = c:\\flexlm\\license.dat; SNPSLMD\_LICENSE\_FILE = c:\\flexlm\\license.dat; 完成！