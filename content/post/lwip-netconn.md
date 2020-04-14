---
title: 'Lwip Netconn'
date: Tue, 12 Jul 2016 11:19:36 +0000
draft: false
tags: ['未分类']
---

从Lwip Netconn接口的代码中，我们可以学到与Lwip 协议栈规范的交互方式和这种接口的封装方法。 这里总结出一个看代码的技巧，第一步先是只看正常处理的流程，忽略异常处理的部分，这样代码就可以非常简洁，可以很快把代码流程搞清楚，第二步再看重要的异常情况的处理过程。 连接的建立过程前面已经简单总结过，这里就只说我们比较关心的recv\_xxx()过程。 recv\_xxx() 函数是在accept\_func里注册的,并且pcb->callback\_arg = new\_netconn。 其回调原型： \[c\] #define TCP\_EVENT\_RECV(pcb,p,err,ret) \\ do { \\ if((pcb)->recv != NULL) { \\ (ret) = (pcb)->recv((pcb)->callback\_arg,(pcb),(p),(err));\\ } else { \\ ... \\ } \\ } while (0) \[/c\] 函数原型： recv\_tcp(void \*arg, struct tcp\_pcb \*pcb, struct pbuf \*p, err\_t err) { struct netconn \*conn; u16\_t len; conn = (struct netconn \*)arg; if (sys\_mbox\_trypost(&conn->recvmbox, p) != ERR\_OK) { ... } 这里从tcpip\_thread抛出的是pbuf。 netconn 接收部分如下： \[c\] netconn\_recv\_data(struct netconn \*conn, void \*\*new\_buf) { void \*buf = NULL; if (sys\_arch\_mbox\_fetch(&conn->recvmbox, &buf, conn->recv\_timeout) == SYS\_ARCH\_TIMEOUT) ... } \[/c\] 这里buf 是void 类型的指针，因为上头可能会用pbuf类型或者netbuf类型来接收邮箱里的pbuf。 netbuf的封装方式：