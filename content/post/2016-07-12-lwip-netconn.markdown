---
author: wuqt
date: 2016-07-12 11:19:36+00:00
draft: false
title: Lwip Netconn
type: post
url: /
categories:
- 未分类
---

从Lwip Netconn接口的代码中，我们可以学到与Lwip 协议栈规范的交互方式和这种接口的封装方法。
这里总结出一个看代码的技巧，第一步先是只看正常处理的流程，忽略异常处理的部分，这样代码就可以非常简洁，可以很快把代码流程搞清楚，第二步再看重要的异常情况的处理过程。
连接的建立过程前面已经简单总结过，这里就只说我们比较关心的recv_xxx()过程。
recv_xxx() 函数是在accept_func里注册的,并且pcb->callback_arg = new_netconn。
其回调原型：
[c]
#define TCP_EVENT_RECV(pcb,p,err,ret)                          \
  do {                                                         \
    if((pcb)->recv != NULL) {                                  \
      (ret) = (pcb)->recv((pcb)->callback_arg,(pcb),(p),(err));\
    } else {                                                   \
...         \
    }                                                          \
  } while (0)
[/c]
函数原型：
recv_tcp(void *arg, struct tcp_pcb *pcb, struct pbuf *p, err_t err)
{
  struct netconn *conn;
  u16_t len;

  conn = (struct netconn *)arg;

  if (sys_mbox_trypost(&conn-;>recvmbox, p) != ERR_OK) {
...


}
这里从tcpip_thread抛出的是pbuf。
netconn 接收部分如下：
[c]
netconn_recv_data(struct netconn *conn, void **new_buf)
{
 void *buf = NULL;
 if (sys_arch_mbox_fetch(&conn->recvmbox, &buf, conn->recv_timeout) == SYS_ARCH_TIMEOUT)
 ...
}
[/c]
这里buf 是void 类型的指针，因为上头可能会用pbuf类型或者netbuf类型来接收邮箱里的pbuf。
netbuf的封装方式：
