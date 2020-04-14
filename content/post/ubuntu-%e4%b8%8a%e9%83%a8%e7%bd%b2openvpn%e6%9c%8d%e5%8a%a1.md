---
title: 'ubuntu 上部署OpenVPN服务'
date: Fri, 19 Aug 2016 15:28:48 +0000
draft: false
tags: ['Network basis', 'OpenVPN ubuntu']
---

缘起： 最近家里的长城宽带通往海外的线路堵得厉害(感觉长城宽带就是GFW的实验场)，用ss翻墙都难连上服务器，apple store也难连上。于是不得不在国内阿里云的服务器上搭一个VPN。不仅可以解决长城宽带的问题，平时手机在外面连接不安全的wifi时也不用担心信息会泄漏了。另外VPN还可以将分布在各处的内网主机组成局域网，VPN服务就相当于是在中间的虚拟路由器。 参考了以上几个链接的博客总结一下安装过程。 [http://www.linuxidc.com/Linux/2014-08/105925p2.htm](http://www.linuxidc.com/Linux/2014-08/105925p2.htm) [http://www.linuxidc.com/Linux/2012-01/51702.htm](http://www.linuxidc.com/Linux/2012-01/51702.htm) [https://help.ubuntu.com/lts/serverguide/openvpn.html](https://help.ubuntu.com/lts/serverguide/openvpn.html) [http://blog.csdn.net/joyous/article/details/8034132](http://blog.csdn.net/joyous/article/details/8034132) OpenVPN 是在TCP/UDP 端口上建立一个安全IP网络通道，支持SSL/TLS 对数据加密、授权等。简单点理解就是在传输层上建立一个虚拟线路，作IP包的交换，相当于一个虚拟网卡。原文是这么说的： \* OpenVPN -- An application to securely tunnel IP networks \* over a single TCP/UDP port, with support for SSL/TLS-based \* session authentication and key exchange, \* packet encryption, packet authentication, and \* packet compression. **一、安装OpenVPN** \[bash\] ~$ apt-get install openvpn dnsmasq \[/bash\] dnsmasq 在用来在vpn路由器功能里面充当域名服务器的作用。有的版本openvpn包含了 easy-rsa ，不用再安装easy-rsa。如果没有再自行安装easy-rsa，它包含生成密钥用的脚本文件。 注：更新本地仓库：apt-get update 安装完查看openvpn相关安装包位置的命令： \[bash\] ~$ dpkg -L openvpn |more \[/bash\] 找到 easy-rsa 所在的文件夹，把它拷到/etc/openvpn/底下。如果没有就另行安装 easy-rsa。 \[bash\] ~$ cp -r /usr/share/doc/openvpn/examples/easy-rsa/2.0/ /etc/openvpn/easy-rsa/ \[/bash\] **二、生成证书和私钥** 包括： 1. 建立一个证书颁发机构(CA)，用来创建其它证书； 2. 为服务端创建一个证书(公钥)和一个私钥； 3. 为客户端创建证书和私钥； 1. 生成CA.key \[bash\] ~$ vim /etc/openvpn/easy-rsa/2.0/vars \[/bash\] 编辑vars文件,看自己需要修改这些东西，后面在生成证书的时候也可以实时输入。 \[code\] export KEY\_COUNTRY="GR" export KEY\_PROVINCE="Central Macedonia" export KEY\_CITY="Thessaloniki" export KEY\_ORG="Parabing Creations" export KEY\_EMAIL="nobody@parabing.com" export KEY\_CN="VPNsRUS" export KEY\_NAME="VPNsRUS" export KEY\_OU="Parabing" export KEY\_ALTNAMES="VPNsRUS \[/code\] \[bash\] # source ./vars NOTE: If you run ./clean-all, I will be doing a rm -rf on /etc/openvpn/easy-rsa/2.0/keys # ./clean-all # ./build-ca \[/bash\] 2. 生成servername.key \[bash\] # ./build-key-server servername \[/bash\] 生成Diffie-Hellman参数 用来在不安全的通信通道里安全的交换密钥。好像用处不大。 \[bash\] # ./build-dh \[/bash\] 3. 生成clientname.key \[bash\] # ./build-key clientname \[/bash\] 所以我们到现在为止已经有如下7个文件了: 它们都储存在/etc/openvpn/easy-rsa/keys， ca.crt – 证书颁发机构(CA)的证书 ca.key – CA的私钥 servername.crt – OpenVPN服务器的证书 servername.key – OpenVPN服务器的私钥 dh1024.pem – Diffie-Hellman参数文件 clientname.crt clientname.key **三、服务端配置** 1. 解压server.conf, # gzip -d ./server.conf.gz # vim server.conf 配置server.conf,这步很重要 \[code\] #根据实际修改这些文件路径: ca /etc/openvpn/keys/ca.crt cert /etc/openvpn/keys/servername.crt key /etc/openvpn/keys/servername.key dh /etc/openvpn/keys/dh1024.pem #根据自己放置证书文件的位置修改好，最好用绝对路径。 协议可以选择udp 或者tcp。 # TCP or UDP server? proto tcp ;proto udp #在配置文件的末尾，我们添加下面这两行: push "redirect-gateway def1" push "dhcp-option DNS 10.8.0.1" \[/code\] 最后这两行指示客户端用OpenVPN作为默认的网关，并用10.8.0.1作为DNS服务器。注意10.8.0.1是OpenVPN启动时自动创建的隧道接口的IP。如果客户用别的域名解析服务，那么我们就得提防不安全的DNS服务器。为了避免这种泄露，我们建议所有OpenVPN客户端使用10.8.0.1作为DNS服务器。 \[code\] 另外可以根据需要开启 client-to-client 让客户端之间可以互相访问 再开启 duplicate-cn 让一个证书支持多地同时登陆 把日志打开，万一不成功可方便调试 log openvpn.log log-append openvpn.log \[/code\] 再打开ip转发： \[bash\] #开启ip\_forward # vim /etc/sysctl.conf # sysctl -p /etc/sysctl.conf \[/bash\] **开启服务器** \[bash\] # service openvpn start \* Starting virtual private network daemon(s)... \* Autostarting VPN 'server' # netstat -nl Active Internet connections (servers and established) Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program name udp 0 0 10.8.0.1:123 0.0.0.0:\* 962/ntpd udp 0 0 172.16.0.1:123 0.0.0.0:\* 962/ntpd udp 0 0 127.0.0.1:123 0.0.0.0:\* 962/ntpd udp 0 0 0.0.0.0:123 0.0.0.0:\* 962/ntpd udp 0 0 0.0.0.0:1194 0.0.0.0:\* 5940/openvpn udp6 0 0 :::123 :::\* 962/ntpd \[/bash\] 2. 配置dnsmasq.conf \[code\] #修改这一行: listen-address=127.0.0.1,10.8.0.1 #uncommon 下一行: bind-interfaces 启动dns \[/code\] \[bash\] # service dnsmasq restart \[/bash\] **四、配置ip\_tables** 要配置好路由转发客户端才能通过VPN上网。 \[bash\] echo 1 > /proc/sys/net/ipv4/ip\_forward # iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT # iptables -A FORWARD -s 10.8.0.0/24 -j ACCEPT # iptables -A FORWARD -j REJECT # iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE 希望每次Ubuntu启动的时候，这些规则都好用，可以把它们加到/etc/rc.local里。 \[/bash\] **五、客户端配置** \[bash\]iptab # cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf /etc/openvpn/ # vim client.conf \[/bash\] 在ssh中可以用scp 命令将远程服务器上的文件拷贝至本地 \[bash\] scp root@serverip:/etc/openvpn/client.conf /home/ \[/bash\] 客户端的密钥和证书也一样下载下来。 编辑client.conf remote my-server-1 1194 my-server-1改成 服务器的IP 还要修改两行: cert client.crt key client.key 改成自己生成的证书名。 windows 客户端可以用 openvpn-GUI 将证书和配置文件放置config文件夹下，.conf文件重命名为.ovpn即可。 连接成功后查看公网ip，如果是服务器的IP就成功了！ 终于大功告成！ 再配置一个客户端给家里电脑使用，这样就可以将它跟公司计算机连在一个局域网了。 使用国内服务器的VPN + 海外VPS上的Shadowsocks上网，简直Perfect！ 后记，如果要在手机上用openvpn时，可以将key和crt，放到client.ovpn文件里面，分别贴到 \[code\] <ca></ca> <key></key> <cert></cert> \[/code\] 中间即可。openvpn国内app store 已经下架了，可以上国外的store上下。

> **用户名密码的配置方式** [http://www.cnblogs.com/electron/p/3488033.html](http://www.cnblogs.com/electron/p/3488033.html) **\[转\]OpenVPN使用用户名/密码验证方式** OpenVPN推荐使用证书进行认证，安全性很高，但是配置起来很麻烦。还好它也能像pptp等vpn一样使用用户名/密码进行认证。 不管何种认证方式，服务端的ca.crt, server.crt, server.key, dh1024.pem这四个证书都是要的。使用username/passwd 方式，你需要在服务器配置文件中加入以下语句，取消客户端的证书认证： client-cert-not-required 然后加入auth-user-pass-verify，开启用户密码脚本： auth-user-pass-verify /etc/openvpn/checkpsw.sh via-env 加入script-security消除以下警告 script-security 3 system checkpsw.sh脚本可以通过网络获取 wget http://openvpn.se/files/other/checkpsw.sh checkpsw.sh默认从文件/etc/openvpn/psw-file中读取用户名密码。 psw-file中一行是一个账号，用户名和密码之间用空格隔开 username password 到此服务端就配置完成了。 客户端配置文件中去掉于证书相关的配置，加入 auth-user-pass 打开用户名密码验证。 可以加入auth-nocache可以在断线后防止内存中保存用户名和密码来提高安全性。 这样客户端就配好。 下面提供一个我的服务端配置文件。Linux使用该配置文件作为服务端，ROS的openvpn客户端是可以连接上的。 复制代码 port 1194 proto tcp dev tap #不要求客户端有证书 client-cert-not-required username-as-common-name script-security 3 system #使用脚本验证密码 auth-user-pass-verify /etc/openvpn/checkpsw.sh via-env ca /etc/openvpn/keys/ca.crt cert /etc/openvpn/keys/server.crt key /etc/openvpn/keys/server.key dh /etc/openvpn/keys/dh1024.pem server 10.8.6.0 255.255.255.0 #保存已有的用户和ip的对应关系 ifconfig-pool-persist ipp.txt #允许客户端之间互访 client-to-client keepalive 10 120 user nobody group nogroup persist-key persist-tun #保存日志 status openvpn-status.log #日志冗余级别 verb 3 复制代码