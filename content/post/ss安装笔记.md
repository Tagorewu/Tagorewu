---
title: 'ss安装笔记'
date: Fri, 04 Nov 2016 09:54:02 +0000
draft: false
categories:
  - "Network"
tags: ['未分类']
---

现在的上网安全性越来越差了，感觉随时有人在监视你上网。电信送的政企网关更是漏洞百出，dns经常被劫持，而且电信留有后门，非常不安全。再加上之前出现的同事邮件被监视，诈骗邮件、短信等等的问题，公司的网络真的是问题很大。 之前部署的openvpn服务在手机端用，似乎dns有问题不能用，不用dns的服务则可正常使用。而PC端使用OpenVPN无任何问题。所以打算在国内的服务器上也部署一个SS来上网。 pip安装方式简单记录下： \[bash\] #apt-get intsall python-pip #pip install shadowsocks #vim /etc/shadowsocks.json 如下 { "server":"my\_server\_ip", "server\_port":8388, "local\_address": "127.0.0.1", "local\_port":1080, "password":"mypassword", "timeout":300, "method":"aes-256-cfb", "fast\_open": false } #ssserver -c /etc/shadowsocks.json -d start 若要开机启动则将本条添加到 /etc/rc.local 里 \[/bash\] 由于在 pip install 的时候出了问题，估计是国内访问pypi资源很慢，安装失败。于是就打算使用源码安装方式。 1. 用scp 拷贝源码到服务器。 2. \[bash\] #python setup.py build #python setup.py install #等待安装完成，之后就跟上面的一样配置就可以了。 \[/bash\] 要赶紧把ss的源码好好学学，不然哪天翻不出去也要会自力更生，嘿嘿。 特别 github 还有python大牛源码解析的版本和其它衍生版本可以下哦！这里不贴出来，以免被封。 tips: 对于长城宽带，ss代理若使用常用的443端口经常遇到出不去的情况，可以用telnet 通一下443端口，然后就神奇的可以出去了。这也是无意中发现的，长宽真是个神奇的存在！电信就很少遇到这个问题，不过有极少情况可能是因为找不到路由，可以tracert 一下服务器应该就能出去了。