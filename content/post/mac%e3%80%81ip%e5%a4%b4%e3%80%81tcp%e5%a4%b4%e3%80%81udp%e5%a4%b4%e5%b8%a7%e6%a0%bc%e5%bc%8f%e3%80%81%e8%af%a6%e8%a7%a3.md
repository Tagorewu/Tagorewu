---
title: 'Ethernet、IP、TCP、UDP帧头格式、详解'
date: Thu, 03 Mar 2016 01:57:48 +0000
draft: false
tags: ['mac', 'Network basis', 'tcp/ip', 'udp']
---

引用：http://zoufengfu168.blog.163.com/blog/static/5461055200991333616451/ 一、MAC帧头定义 \[c\] typedef struct \_MAC\_FRAME\_HEADER { char m\_cDstMacAddress\[6\]; //目的mac地址 char m\_cSrcMacAddress\[6\]; //源mac地址 /\* 如0x0800代表上一层是IP协议，0x0806为arp, 0x88a4为Ethercat \*/ short m\_cType; 　　　　　//上一层协议类型， }\_\_attribute\_\_((packed))MAC\_FRAME\_HEADER,\*PMAC\_FRAME\_HEADER; Etherne II 长度为14 byte; typedef struct \_MAC\_FRAME\_TAIL { unsigned int m\_sCheckSum; //数据帧尾校验和 }\_\_attribute\_\_((packed))MAC\_FRAME\_TAIL, \*PMAC\_FRAME\_TAIL; \[/c\] 补充VLAN 和ARP： \[c\] // #define ETHTYPE\_ARP 0x0806U #define ETHTYPE\_IP 0x0800U #define ETHTYPE\_VLAN 0x8100U #define ETHTYPE\_PPPOEDISC 0x8863U /\* PPP Over Ethernet Discovery Stage \*/ #define ETHTYPE\_PPPOE 0x8864U /\* PPP Over Ethernet Session Stage \*/ /\*\* VLAN header inserted between ethernet header and payload \* if 'type' in ethernet header is ETHTYPE\_VLAN. \* See IEEE802.Q \*/ struct eth\_vlan\_hdr { PACK\_STRUCT\_FIELD(u16\_t prio\_vid); PACK\_STRUCT\_FIELD(u16\_t tpid); } PACK\_STRUCT\_STRUCT; /\*\* the ARP message, see RFC 826 ("Packet format") \*/ struct etharp\_hdr { PACK\_STRUCT\_FIELD(u16\_t hwtype); PACK\_STRUCT\_FIELD(u16\_t proto); PACK\_STRUCT\_FIELD(u8\_t hwlen); PACK\_STRUCT\_FIELD(u8\_t protolen); PACK\_STRUCT\_FIELD(u16\_t opcode); PACK\_STRUCT\_FIELD(struct eth\_addr shwaddr); PACK\_STRUCT\_FIELD(struct ip\_addr2 sipaddr); PACK\_STRUCT\_FIELD(struct eth\_addr dhwaddr); PACK\_STRUCT\_FIELD(struct ip\_addr2 dipaddr); } PACK\_STRUCT\_STRUCT; \[/c\] 二、IP头结构的定义 ![](http://my.csdn.net/uploads/201205/25/1337910943_1128.jpg) \[c\] typedef struct \_IP\_HEADER { /\* 如：常见为0x45,即IPv4,不带选项的5字(32bits)包头。带选项则最长为15字。\*/ char m\_cVersionAndHeaderLen; // 版本信息(前4位)，头长度(后4位)。 char m\_cTypeOfService; 　　　　　 // 服务类型8位 short m\_sTotalLenOfPacket; 　　　　//数据包长度 short m\_sPacketID; 　　　　　　　 //数据包标识 short m\_sSliceinfo; 　　　　　　　 //分片使用 char m\_cTTL; 　　　　　　　　　　//存活时间 char m\_cTypeOfProtocol; 　　　　　 //协议类型 short m\_sCheckSum; 　　　　　　 //校验和 unsigned int m\_uiSourIp; 　　　　　//源ip unsigned int m\_uiDestIp; 　　　　　//目的ip } \_\_attribute\_\_((packed))IP\_HEADER, \*PIP\_HEADER ; \[/c\] 常用的几个协议号： 1 ICMP 2 IGMP 6 TCP 17 UDP 88 IGRP 89 OSPF 三、tcp头结构定义 ![](http://my.csdn.net/uploads/201205/25/1337910956_9817.jpg) \[c\] typedef struct \_TCP\_HEADER { short m\_sSourPort; 　　　　　　// 源端口号16bit short m\_sDestPort; 　　　　　　 // 目的端口号16bit unsigned int m\_uiSequNum; 　　// 序列号32bit unsigned int m\_uiAcknowledgeNum; // 确认号32bit short m\_sHeaderLenAndFlag; 　　// 前4位：TCP头长度；中6位：保留；后6位：标志位 short m\_sWindowSize; 　　　　　// 窗口大小16bit short m\_sCheckSum; 　　　　　 // 检验和16bit short m\_surgentPointer; 　　　　 // 紧急数据偏移量16bit }\_\_attribute\_\_((packed))TCP\_HEADER, \*PTCP\_HEADER; typedef struct \_TCP\_OPTIONS { char m\_ckind; char m\_cLength; char m\_cContext\[32\]; }\_\_attribute\_\_((packed))TCP\_OPTIONS, \*PTCP\_OPTIONS; \[/c\] 四、UDP头结构的定义 ![](http://my.csdn.net/uploads/201205/25/1337910963_6079.jpg) \[c\] typedef struct \_UDP\_HEADER { unsigned short m\_usSourPort; 　　　// 源端口号16bit unsigned short m\_usDestPort; 　　　// 目的端口号16bit unsigned short m\_usLength; 　　　　// 数据包长度16bit unsigned short m\_usCheckSum; 　　// 校验和16bit }\_\_attribute\_\_((packed))UDP\_HEADER, \*PUDP\_HEADER; \[/c\] ==== http://www.cnblogs.com/li-hao/archive/2011/12/07/2279912.html ------------------------------------------------------------------------------------------------------------------------------------- tcp、ip、udp头部格式 2.2　TCP/IP报文格式 1、IP报文格式 IP协议是TCP/IP协议族中最为核心的协议。它提供不可靠、无连接的服务，也即依赖其他层的协议进行差错控制。在局域网环境，IP协议往往被封装在以太网帧（见本章1.3节）中传送。而所有的TCP、UDP、ICMP、IGMP数据都被封装在IP数据报中传送。如图2-3所示： ![](http://my.csdn.net/uploads/201205/25/1337910967_8133.jpg) 图2-3　 TCP/IP报文封装 ![](http://my.csdn.net/uploads/201205/25/1337910978_9542.jpg) 图2-4是IP头部（报头）格式：（RFC 791）。   图2-4　 IP头部格式 其中： ●版本（Version）字段：占4比特。用来表明IP协议实现的版本号，当前一般为IPv4，即0100。 ●报头长度（Internet Header Length，IHL）字段：占4比特。是头部占32比特的数字，包括可选项。普通IP数据报（没有任何选项），该字段的值是5，即160比特=20字节。此字段最大值为60字节。 ●服务类型（Type of Service ，TOS）字段：占8比特。其中前3比特为优先权子字段（Precedence，现已被忽略）。第8比特保留未用。第4至第7比特分别代表延迟、吞吐量、可靠性和花费。当它们取值为1时分别代表要求最小时延、最大吞吐量、最高可靠性和最小费用。这4比特的服务类型中只能置其中1比特为1。可以全为0，若全为0则表示一般服务。服务类型字段声明了数据报被网络系统传输时可以被怎样处理。例如：TELNET协议可能要求有最小的延迟，FTP协议（数据）可能要求有最大吞吐量，SNMP协议可能要求有最高可靠性，NNTP（Network News Transfer Protocol，网络新闻传输协议）可能要求最小费用，而ICMP协议可能无特殊要求（4比特全为0）。实际上，大部分主机会忽略这个字段，但一些动态路由协议如OSPF（Open Shortest Path First Protocol）、IS-IS（Intermediate System to Intermediate System Protocol）可以根据这些字段的值进行路由决策。 ●总长度字段：占16比特。指明整个数据报的长度（以字节为单位）。最大长度为65535字节。 ●标志字段：占16比特。用来唯一地标识主机发送的每一份数据报。通常每发一份报文，它的值会加1。 ●标志位字段：占3比特。标志一份数据报是否要求分段。 ●段偏移字段：占13比特。如果一份数据报要求分段的话，此字段指明该段偏移距原始数据报开始的位置。 ●生存期（TTL：Time to Live）字段：占8比特。用来设置数据报最多可以经过的路由器数。由发送数据的源主机设置，通常为32、64、128等。每经过一个路由器，其值减1，直到0时该数据报被丢弃。 ●协议字段：占8比特。指明IP层所封装的上层协议类型，如ICMP（1）、IGMP（2） 、TCP（6）、UDP（17）等。 ●头部校验和字段：占16比特。内容是根据IP头部计算得到的校验和码。计算方法是：对头部中每个16比特进行二进制反码求和。（和ICMP、IGMP、TCP、UDP不同，IP不对头部后的数据进行校验）。 ●源IP地址、目标IP地址字段：各占32比特。用来标明发送IP数据报文的源主机地址和接收IP报文的目标主机地址。 可选项字段：占32比特。用来定义一些任选项：如记录路径、时间戳等。这些选项很少被使用，同时并不是所有主机和路由器都支持这些选项。可选项字段的长度必须是32比特的整数倍，如果不足，必须填充0以达到此长度要求。 2、TCP数据段格式 TCP是一种可靠的、面向连接的字节流服务。源主机在传送数据前需要先和目标主机建立连接。然后，在此连接上，被编号的数据段按序收发。同时，要求对每个数据段进行确认，保证了可靠性。如果在指定的时间内没有收到目标主机对所发数据段的确认，源主机将再次发送该数据段。 如图2-5所示，是TCP头部结构（RFC 793、1323）。 ![](http://my.csdn.net/uploads/201205/25/1337910993_8515.jpg) 图2-5　 TCP头部结构 ●源、目标端口号字段：占16比特。TCP协议通过使用"端口"来标识源端和目标端的应用进程。端口号可以使用0到65535之间的任何数字。在收到服务请求时，操作系统动态地为客户端的应用程序分配端口号。在服务器端，每种服务在"众所周知的端口"（Well-Know Port）为用户提供服务。 ●顺序号字段：占32比特。用来标识从TCP源端向TCP目标端发送的数据字节流，它表示在这个报文段中的第一个数据字节。 ●确认号字段：占32比特。只有ACK标志为1时，确认号字段才有效。它包含目标端所期望收到源端的下一个数据字节。 ●头部长度字段：占4比特。给出头部占32比特的数目。没有任何选项字段的TCP头部长度为20字节；最多可以有60字节的TCP头部。 ●标志位字段（U、A、P、R、S、F）：占6比特。各比特的含义如下： ◆URG：紧急指针（urgent pointer）有效。 ◆ACK：确认序号有效。 ◆PSH：接收方应该尽快将这个报文段交给应用层。 ◆RST：重建连接。 ◆SYN：发起一个连接。 ◆FIN：释放一个连接。 ●窗口大小字段：占16比特。此字段用来进行流量控制。单位为字节数，这个值是本机期望一次接收的字节数。 ●TCP校验和字段：占16比特。对整个TCP报文段，即TCP头部和TCP数据进行校验和计算，并由目标端进行验证。 ●紧急指针字段：占16比特。它是一个偏移量，和序号字段中的值相加表示紧急数据最后一个字节的序号。 ●选项字段：占32比特。可能包括"窗口扩大因子"、"时间戳"等选项。 3、UDP数据段格式 UDP是一种不可靠的、无连接的数据报服务。源主机在传送数据前不需要和目标主机建立连接。数据被冠以源、目标端口号等UDP报头字段后直接发往目的主机。这时，每个数据段的可靠性依靠上层协议来保证。在传送数据较少、较小的情况下，UDP比TCP更加高效。 如图2-6所示，是UDP头部结构（RFC 793、1323）： ![](http://my.csdn.net/uploads/201205/25/1337911478_8471.jpg) 图2-6　 UDP数据段格式 ●源、目标端口号字段：占16比特。作用与TCP数据段中的端口号字段相同，用来标识源端和目标端的应用进程。 ●长度字段：占16比特。标明UDP头部和UDP数据的总长度字节。 ●校验和字段：占16比特。用来对UDP头部和UDP数据进行校验。和TCP不同的是，对UDP来说，此字段是可选项，而TCP数据段中的校验和字段是必须有的。 2.3　套接字 在每个TCP、UDP数据段中都包含源端口和目标端口字段。有时，我们把一个IP地址和一个端口号合称为一个套接字（Socket），而一个套接字对（Socket pair）可以唯一地确定互连网络中每个TCP连接的双方（客户IP地址、客户端口号、服务器IP地址、服务器端口号）。 如图2-7所示，是常见的一些协议和它们对应的服务端口号。 ![](http://my.csdn.net/uploads/201205/25/1337911485_7934.jpg) 图2-7　 常见协议和对应的端口号 需要注意的是，不同的应用层协议可能基于不同的传输层协议，如FTP、TELNET、SMTP协议基于可靠的TCP协议。TFTP、SNMP、RIP基于不可靠的UDP协议。 同时，有些应用层协议占用了两个不同的端口号，如FTP的20、21端口，SNMP的161、162端口。这些应用层协议在不同的端口提供不同的功能。如FTP的21端口用来侦听用户的连接请求，而20端口用来传送用户的文件数据。再如，SNMP的161端口用于SNMP管理进程获取SNMP代理的数据，而162端口用于SNMP代理主动向SNMP管理进程发送数据。 还有一些协议使用了传输层的不同协议提供的服务。如DNS协议同时使用了TCP 53端口和UDP 53端口。DNS协议在UDP的53端口提供域名解析服务，在TCP的53端口提供DNS区域文件传输服务。 2.4　TCP连接建立、释放时的握手过程 1、TCP建立连接的三次握手过程 TCP会话通过三次握手来初始化。三次握手的目标是使数据段的发送和接收同步。同时也向其他主机表明其一次可接收的数据量（窗口大小），并建立逻辑连接。这三次握手的过程可以简述如下： ●源主机发送一个同步标志位（SYN）置1的TCP数据段。此段中同时标明初始序号（Initial Sequence Number，ISN）。ISN是一个随时间变化的随机值。 ●目标主机发回确认数据段，此段中的同步标志位（SYN）同样被置1，且确认标志位（ACK）也置1，同时在确认序号字段表明目标主机期待收到源主机下一个数据段的序号（即表明前一个数据段已收到并且没有错误）。此外，此段中还包含目标主机的段初始序号。 ●源主机再回送一个数据段，同样带有递增的发送序号和确认序号。 至此为止，TCP会话的三次握手完成。接下来，源主机和目标主机可以互相收发数据。整个过程可用图2-8表示。 ![](http://my.csdn.net/uploads/201205/25/1337911008_5159.jpg) 图2-8　 TCP建立连接的三次握手过程 2、TCP释放连接的四次握手过程 TCP连接的释放需要进行四次握手，步骤是： ●源主机发送一个释放连接标志位（FIN）为1的数据段发出结束会话请求