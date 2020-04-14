---
title: '[转]各种工业以太网比较（EtherCAT,EtherNet/IP,ProfiNet,Modbus-TCP,Powerlink）'
date: Wed, 27 Jan 2016 06:15:31 +0000
draft: false
tags: ['Ethernet /IP', '工业以太网', '未分类']
---

EtherCAT（以太网控制自动化技术）是一个以以太网为基础的开放架构的现场总线系统，EterCAT名称中的CAT为ControlAutomation Technology（控制自动化技术）首字母的缩写。最初由德国倍福自动化有限公司(Beckhoff AutomationGmbH)研发。EtherCAT为系统的实时性能和拓扑的灵活性树立了新的标准，同时，它还符合甚至降低了[现场总线](http://baike.baidu.com/view/15180.htm)的使用成本。EtherCAT的特点还包括高精度设备同步，可选线缆冗余，和功能性安全协议(SIL3)。   **Ethernet/IP**是一个面向工业自动化应用的工业[应用层](http://baike.baidu.com/view/239619.htm)协议。它建立在标准UDP/IP与TCP/IP协议之上，利用固定的以太网硬件和软件，为配置、访问和控制工业自动化设备定义了一个[应用层](http://baike.baidu.com/view/239619.htm)协议西蒙公司开发  

PROFINET由PROFIBUS国际组织（PROFIBUS International，PI）推出，是新一代基于工业以太网技术的自动化[总线标准](http://baike.baidu.com/view/672343.htm)。作为一项战略性的技术创新，[PROFINET](http://baike.baidu.com/view/835484.htm)为自动化通信领域提供了一个完整的网络解决方案，囊括了诸如实时以太网、[运动控制](http://baike.baidu.com/view/1501678.htm)、分布式自动化、故障安全以及网络安全等当前自动化领域的热点话题，并且，作为跨供应商的技术，可以完全兼容[工业以太网](http://baike.baidu.com/view/568270.htm)和现有的[现场总线](http://baike.baidu.com/view/15180.htm)（如[PROFIBUS](http://baike.baidu.com/view/14965.htm)）技术，保护现有投资。

PROFINET是适用于不同需求的完整解决方案，其功能包括8个主要的模块，依次为实时通信、分布式现场设备、[运动控制](http://baike.baidu.com/view/1501678.htm)、分布式自动化、网络安装、IT标准和信息安全、故障安全和[过程自动化](http://baike.baidu.com/view/5981041.htm)。

  MODBUS/TCP是简单的、中立厂商的用于管理和控制自动化设备的MODBUS系列通讯协议的派生产品。显而易见，它覆盖了使用TCP/IP协议的 “Intranet”和“Internet”环境中MODBUS 报文的用途。协议的最通用用途是为诸如PLC’s，I/O模块，以及连接其它简单域总线或I/O模块的网关服务的。 MODBUS/TCP协议是作为一种（实际的）自动化标准发行的。既然MODBUS已经广为人知，该规范只将别处没有收录的少量信息列入其中。然而，本规范力图阐明MODBUS中哪种功能对于普通自动化设备的互用性有价值，哪些部分是MODBUS作为可编程的协议交替用于PLC’s的“多余部分”。 它通过将配套报文类型“一致性等级”，区别那些普遍适用的和可选的，特别是那些适用于特殊设备如PLC’s的报文。   [POWERLINK=CANopen+Ethernet](http://baike.baidu.com/view/5997651.htm?fr=aladdin#1_1) 鉴于[以太网](http://baike.baidu.com/view/848.htm)的蓬勃发展和CANopen在自动化领域里的广阔应用基础，EthernetPOWERLINK 融合了这两项技术的优点和缺点，即拥有了Ethernet的高速、开放性接口，以及CANopen在工业领域良好的SDO 和PDO 数据定义，在某种意义上说POWERLINK就是Ethernet 上的CANopen，[物理层](http://baike.baidu.com/view/239585.htm)、[数据链路层](http://baike.baidu.com/view/239592.htm)使用了Ethernet介质，而[应用层](http://baike.baidu.com/view/239619.htm)则保留了原有的SDO和PDO对象字典的结构 虽然这些工业以太网都是国际标准，但是指的是IEC 61784里的标准，但是这些工业以太网不都是标准的以太网。即这些工业以太网并不都是符合IEEE802.3U的标准，这当中只有Modbus-TCP和EtherNet/IP是符合IEEE802.3U的，只有符合IEEE802.3U标准的，才能与IT和以太网将来的发展相兼容。而不符合IEEE802.3U标准的，基本上可以讲不是以太网，它们都对以太网进行了修改，或者是硬件或者是软件，已经不是以太网了。     各种工业以太网的区别其实主要就是协议的区别，其中最主要的还是应用层协议的区别，我们知道，按照ISO的参考模型，网络被划分为7层。   a.     Modbus TCP和EtherNet/IP的区别主要是应用层不相同，ModbusTCP的应用层采用Modbus协议，而EtherNet/IP采用CIP协议，这两种工业以太网的数据链路层采用的是CSMA/CD，因此是标准的以太网，另外，这两种工业以太网的网络层和传输层采用TCP/IP协议族。还有一个区别是，Modbus协议中迄今没有协议来完成功能安全、高精度同步和运功控制等，而EtherNet/IP有CIPSafety、CIP Sync和CIP Motion来完成上述功能，所以才有Schneider加入ODVA，成为ODVA的核心成员来推广EtherNet/IP。由于这两种网络都是标准的TCP/IP以太网，所以所有标准以太网节点都可以接入这两种网络。 b.     b. 至于EthernetPowerLink(EPL), Ethernet PowerLink就是个怪胎，PowerLink虽然在物理层和数据链路层还是采用标准的以太网，但是它又添加了另一个数据链路层，此EPL数据链路层在结构上为于以太网数据链路层之上。我们知道数据链路层的一个子层的MAC(介质访问)层的作用是\[color=#FF0000\]决定哪一个节点可以占有总线，也即决定哪个节点一个发送数据\[/color\]。所以本来由以太网的数据链路层来决定哪一个节点占用总线，现在它被位于它之上的EPL数据链路层给架空了，由这个EPL数据链路层通过软件的方式来决定哪个节点发送数据。所有在这样的一个EPL工业以太网系统中，不能使用交换机，只能使用HUB，所以对100M的网络，EPL总的带宽是小于100m,一盘情况下只有40－50M，而如果采用交换机的工业以太网，它的带宽可以达到大几百M,另外在EPL网络上，所有的节点都要实现EPL数据链路。没有实现EPL数据链路层的节点不能接入此网络。 c. PROFINET分为原来划分为v1,v2,v3，现在一般称为ProfiNetCBA、ProfiNet IO和ProfiNet IRT.也就是通过以太网来实现对等通讯、实时控制和运动控制。v1采用TCP/IP协议，采用标准的以太网，而V2和V3不采用tcp/ip协议，这两种都绕过tcp/ip协议，采用另外的网络层和传输层协议，开发ProfiNet采用开发人员人员认为tcp/ip协议增加了数据在网络中的传输延迟，其实这是一种误解，据美国密歇根大学的教授研究后认为数据在TCP/IP中的传输延迟很小，他们研究得出数据在经过TCP,IP栈时延迟只有不到100微秒，如果采用UDP/IP时就更小，同时他们研究也得出数据在不同应用层延时比较大，不同的协议延迟不一样，但是相差不是很大，从200us-800us不等，他们经过实验后认为以太网的基础设施(指交换机、网卡等）和TCP/IP协议并不是影响工业以太网实时性的主要原因，而认为应用层协议才是主要原因。所以密歇根大学的教授认为绕开TCP/IP协议没有丝毫的意义，反而由于缺少了TCP/IP协议，使得设备也就缺少了IT功能，与其它现场总线没有区别。 ProfiNet V3就更特别了，它不完全采用标准以太网的数据链路层，有一不时间采用以太网的数据链路层(CSMA/CD)，而另外一部分时间采用自己的数据链路层，通过一个高精度的时间来完成。所以ProfiNet V3也就不是标准的以太网了，也就给Profinet v3带来如下的问题：不能采用标准的交换机、不能采用标准的以太网芯片、与企业网相连可能会出现问题，与标准以太网相连还要特殊的网关、添加和删除一个节点都需要重新组态网络和重新启动网络、至今没有千兆网络，还有最重要的是，当标准以太网以后发展了后，它不能与标准以太网相兼容，不具有将来以太网所应具有的功能。 d. EtherCat这种工业以太网也很奇怪，它们不使用标准的芯片，一般不使用交换机，软件也不是标准的，对以太网的数据帧进行了一些修改，我们知道一个数据帧只有一个源节点，但是对于EtherCat一个数据可能有多个源节点，即一个数据是由多个节点发送的数据组合而成的。所以对于这样的网络，标准的以太网设备也不能接入这样的网络。   我认为Ethernet/IP和ProfiNet这两种工业以太网都适合各个行业，并不象heidai讲的应用的行业不一样。首先这两种工业以太网都用于传输非实时数据，还可传输实时数据，即可以用于离散控制，也可用于过程控制(当然现在还不能用于本安应用)。其次，这两种工业以太网都可用于网络功能安全传输，Ethernet/IP有CIP Safety协议，而ProfiNet有Profisafe协议,还有在运动控制方面ProfiNet有 ProfiNet IRT，而EtherNet/IP则有CIP Safety,二者都可以用于中高端的运动控制。最后两者都有基于IEEE1588的高精度时钟同步。而Modbus TCP,EtherCat和PowerLink,都只能完成部分控制任务，如Modbus TCP一般只作常规IO实时和非实时数据。而EtherCat和PowerLink则更象是为运动控制而开发的，这二者好像没有功能安全、在PLC和DCS控制方面也没有得到大自动化公司的支持，况且这两者又对以太网进行修改，一个在软件，另一个在软件和硬件方面都进行了修改，都不能兼容标准的以太网设备，个人认为这样做得不偿失，为满足运动控制而不能兼容已有的标准的以太网设备而开发的工业以太网并不是以太网，与其说是工业以太网还不如说是另一种现场总线。 我认为工业以太网的竞争将会在Ethernet/IP和ProfiNet间进行，而其它工业以太网都是这两者的陪衬，将会逐渐退出市场。 EtherNet/IP以后将由罗克韦尔自动化、Omron、施耐德和思科公司来推动，而ProfiNet将由业界老大西门子公司带领一些小公司去奋斗，由国内PLC厂商中的老二、老三和老五对老大，不知谁将引导未来。 其实，工业以太网里还有几个怪胎，举两个例吧： SynqNet: 丹纳赫主导的，几乎只用在运动控制，而且据说只用在了半导体机械行业（奇怪的是，不才也搞半导体机械很久了，却从来没看到过SynqNet，孤陋寡闻啊）。只用了以太网的硬件，完全和我们平常说的以太网没有任何关系，连MAC层都没有。当然如此运用，速度性能当然好，但未来难说。 Sercos III: 光纤SercosII的新一代以太网版本，背后推手是博世力士乐，只用在运动控制。也基本上是只用了以太网底层硬件，系统里竟然连switch都不允许用。速度当然快，但只比SercosII快了一倍。估计用了SercosII的用户，谁会去更新到一个没快了多少的新系统啊，还没问世，就已经不被业界看好了。 我个人认为，最后一定是大西洋两岸的两大巨人之间的角力，就像以前的现场总线战争，最后还不是Profibus和DeviceNet，别的都只能当陪衬的角色？ 当然，现在大家都在看中国这个大西洋两岸以外的单一最大市场，中国把砝码放在谁这一边，可能会使天平倾斜一点。但最后，肯定两者都会存在的。我个人认为，咱们应该选Ethernet/IP这一边站 中国用户和制造商应选择Ethernet/IP还是ProfiNet，各人的看法有所不同，不过我认为firstrazor所说的没错，有于ProfiNet采用了专门的芯片、网卡、交换机等以太网基础设施，虽然ProfiNet应用层协议是公开的，但这些芯片却是专用，国内的制造商要想开发符合ProfiNet标准的设备，确要依赖于这些芯片，受制于提供芯片的公司，也就是西门子公司，因此可以将ProfiNet并不是完全开放的。而相反，Ethernet/IP不论是在软件还是硬件上都是标准和开放的，国内的工业以太网制造商还是选择EtherNet/IP为好，至于最终用户的选择，当然是从可靠性、价格、兼容性和可替换性方面考虑，可靠性方面，二者没有明显区别，在其它方面Ethernet/IP具有明显的优势