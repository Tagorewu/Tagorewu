---
author: wuqt
date: 2016-07-06 10:07:02+00:00
draft: false
title: LwIP TCP Layer Simple analysis
type: post
url: /
categories:
- LwIP
tags:
- Lwip
---

LwIP Version : 1.4.1;
这里分享一些阅读LwIP协议栈的心得。
有个经验: 源代码才是最权威的资料。
若想用raw api编程，可以参考httpd_raw的代码。若涉及进程间通讯，请参考netconn接口，避免在其它进程内调用非Thead-Safe的函数。 
下面说说对一些函数及过程的认识。
(1)** xxx_accept(void *arg, struct tcp_pcb *pcb, err_t err); **
这个函数的参数注意点: 回调原型：pcb->accept(pcb->callback_arg, pcb, err);
@*arg: 这里协议栈传递过来的是lpcb，即监听pcb，不要认为和第二个参数相同。因为在tcp_listen_input()里，当有新连接请求到达时会新建一个npcb并将其加入到tcp_active_list里，在初始化npcb时，继承的是lpcb的callback_arg, 而这个callback_arg在新建监听链接时将其赋为lpcb。 
@*pcb: 这里是传递进新创建的pcb。在创建APP过程中，需给其分配新的argument, 指定xxx_recv(),xxx_err(), xxx_poll(), xxx_sent()等函数。

(2) struct tcp_pcb ***tcp_listen( struct tcp_pcb *pcb);**

这里传入了一个original pcb，返回一个lpcb，因为listening状态的pcb 只需包含更少的信息。函数内新分配了一个 lpcb, 拷贝必要信息后，将original pcb释放。 对于SO_REUSE选项，在此函数内对比已经在监听List 中的pcb, 允许本地端口号相同，但本地IP号不同的连接可再次监听。

(3) **服务端口监听过程分析** 
**TCP_LISTEN_BACKLOG** 选项

此选项使能 TCP 对监听列表允许监听的数量控制。在tcp_listen_input()函数内，

[c]

...
else if (flags &amp; TCP_SYN) {
LWIP_DEBUGF(TCP_DEBUG, ("TCP connection request %"U16_F" -&gt; %"U16_F".\n", tcphdr-&gt;src, tcphdr-&gt;dest));
#if TCP_LISTEN_BACKLOG
if (pcb-&gt;accepts_pending &gt;= pcb-&gt;backlog) {
LWIP_DEBUGF(TCP_DEBUG, ("tcp_listen_input: listen backlog exceeded for port %"U16_F"\n", tcphdr-&gt;dest));
return ERR_ABRT;
}
#endif /* TCP_LISTEN_BACKLOG */
npcb = tcp_alloc(pcb-&gt;prio);
...
#if TCP_LISTEN_BACKLOG
pcb-&gt;accepts_pending++;
#endif
...
[/c]

当收到SYN帧时，此处对比并限制已经pending 状态的数量，即未被上层accept的连接数量。接下来新建立一个npcb, 并加入active_pcbs 的list中。
tcp_process() 主要处理 active_pcbs 的状态机。经过三次握手协议之后就进入了ESTABLISHED状态。
这里着重分析下tcp_input(pbuf *p, netif *inp);函数。
函数传入的是一个pbuf和netif，传入的参数并无连接的概念，接着在这里就进入了tcp层。tcp层维护着几个list:
[c]
/** An array with all (non-temporary) PCB lists, mainly used for smaller code size */
struct tcp_pcb ** const tcp_pcb_lists[] = {&tcp_listen_pcbs.pcbs, &tcp_bound_pcbs,
  &tcp_active_pcbs, &tcp_tw_pcbs}; 
[/c]
着重研究active_pcbs lists。当确认传入的pbuf属于某个active_pcbs里的一个连接后，就进入tcp_process进行tcp状态机的处理。然后如果存在有效数据，就会调用accept函数。
这里可以得出一些结论：
1. 当每次有新连接请求进入时，都会创建新的pcb进行记录，并将其加入到active_pcbs list里。之后将完成握手过程的pcb传递 accept函数。
2. accept() 表示应用层接收了一个新连接，有个很重要的环节就是为npcb( 即tcp层维护的一系列已经激活了的连接 ) 指定一个新的arg，用来标记各个连接的应用层参数。 在netconn接口的accept_function中，是将其指向了新建的 netconn。此后在recv函数中就会知道数据是从哪个netconn发送过来的，而不去直接操作pcb。
对tcp多连接的认识：
TCP层维护着一个active_pcbs的链表用来记录每一条连接。每次收到新连接请求就会新建一个pcb。
一个netconn 对应着一个pcb。这个由netconn api负责分配
一个sockets 对应着一个netconn。
一个服务只有一个指定的accept函数及一个acceptmbox，是由该服务的listening_pcb指定的，每次调用accept，都会传入一个新的连接。
当用多线程编写应用时，每个连接都有自己的recv函数及recvmbox。一个连接就需要一个线程去check 对应的recvmbox。就好比电话接线员只需一个人，而接听电话就需要一个人负责一条线。
对于简单的应用来说，应用要支持多连接使用RAW API的方式会比较方便。因为它基于事件触发每个回调函数，不需要多任务去查询消息。

(4) tcp连接的管理过程分析

tcp 维护两个timer，都是由tcp_tmr调用：
tcp_fasttmr() /250ms， process refused  data, and sends delayed acks.
tcp_slowtmr() /500ms, call poll; Check PCB stayed in too long in FIN-WAIT-2 / SYN-RCVD / LAST-ACK;Check if KEEPALIVE should be sent;manage TIME-WAIT PCBS.

每个active的连接都有一个Poll函数负责做定时事件的处理，由tcp_slowtmr()调用。Poll() 每500ms*interval调用一次。可以在此函数内发起关闭连接操作tcp_close(pcb)。

(5) tcp_recv()
TCP was designed to be stream oriented. The receiver doesn't know anything about sent messages/packets. All it sees is a simple stream of bytes. 可以从tcp的报头看到，它并不包含用户数据包何时开始何时结束的信息，tcp只是个面向数据流的协议。所以不用纠结tcp何时会调用tcp_recv函数，接收到包就会调用，可能已经包含了多个包。若应用层只接收了第一个包，则后续的包会通过fast_tmr来触发调用tcp_recv()。
  还有一点tcp和udp 不一样的地方，tcp对方的连接信息是保存在tpcb里的，早在accept函数的时候就保存了连接信息，我们要发送信息只需传入tpcb和pbuf就可以了；而udp对方的信息是从接收函数传进来的，并没有保存在pcb里，如果调用udp send函数, 它会从pcb中拿到对方的地址，若这个udp 绑定的 addr是any的话就变成广播发送了，所以通常调用的是udp sendto 函数，从udp recv函数中获取对方的ip和port。
