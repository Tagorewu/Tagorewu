---
title: "trojan-gfw 安装"
description: ""
date: "2020-07-18T22:42:21+08:00"
thumbnail: ""
categories:
  - "Web"
tags:
  - "trojan-gfw"
---

   [trojan-gfw](https://github.com/trojan-gfw/trojan/) 是一种科学上网代理软件，在外部看来它是一个正常的https网站，然而加密流量会伪装在其中。trojan 现在用的人还比较少，不容易被针对。不过很多时候服务器的443端口也容易被gfw 封锁。

本文简要记录trojan-gfw 的部署过程。

reference:

 https://trojan-tutor.github.io/2019/04/10/p41.html

#### 原理图

![原理图](/images/uploads/2020/trojan_sch.svg)

简要说明下原理，外部https 流量通过443端口输入，trojan 监听443端口，若是正常https 流量则转发至127.0.0.1:80 端口的 nginx 服务器。若是trojan 流量则由trojan 进行代理。

外部访问80 端口的流量由 nginx 转成https 请求，跟正常https 网站一样。



*   由于我的服务器只用来科学上网，安全性要求不是很高，方便起见安装时全用root 账户，这样可以省去很多权限引起的问题。
*   系统版本 ubuntu1604.
*   以下域名用 <example.com>表示 ，请替换成你的域名。

### nginx 安装

- 把/etc/nginx/sites-enable/default 这个默认网站删掉。

- 创建伪装网站

  ```bash
  vim /etc/nginx/sites-available/<example.com>
  ```

  - 网站配置文件
    - @line 4 & 17 : 把<example.com> 改成你的域名。
    - @line 7 : proxy_pass 指向一个没有敏感信息的网站, 这里用了 ieee.org。(原作者用的是ietf.org，好像现在网站禁止转发了。。)
    - @line 15 : 把<10.10.10.10> 改成你的服务器IP。

  ```
  server {
      listen 127.0.0.1:80 default_server;
  
      server_name <example.com>;
  
      location / {
          proxy_pass https://www.ieee.org;
      }
  
  }
  
  server {
      listen 127.0.0.1:80;
  
      server_name <10.10.10.10>;
  
      return 301 https://<example.com>$request_uri;
  }
  
  server {
      listen 0.0.0.0:80;
      listen [::]:80;
  
      server_name _;
  
      location / {
          return 301 https://$host$request_uri;
      }
  
      location /.well-known/acme-challenge {
         root /var/www/acme-challenge;
      }
  }
  
  
  ```

  简要说明一下，

  - server 1 处理从trojan 转发过来的通过域名访问的流量。

  - server 2 处理从trojan 转发过来的通过IP 访问的流量， 转成至域名访问。

  - server 3  监听外部 80 端口，转成https请求。.well-known 是给申请证书准备的。

- 使能/开启网站

  ```bash
  ln -s /etc/nginx/sites-available/<example.com> /etc/nginx/sites-enable/
  # 重启nginx
  service nginx restart
  ```

  

### 证书配置

​	我们使用 let's encrypt 获取证书。

- 创建证书文件夹

  ```bash
  mkdir -p /etc/letsencrypt/live
  ```

- 安装 acme.sh 自动管理CA 证书脚本

  ```bash
  curl  https://get.acme.sh | sh
  ```

- 申请证书

  ```bash
  acme.sh --issue -d <example.com> -w /var/www/acme-challenge
  ```

- 开启自动更新证书，这样一般就不用太管服务器。

  ```bash
  acme.sh  --upgrade  --auto-upgrade
  ```

  

### trojan-gfw 安装

* 使用一键安装脚本安装

  ```bash
  sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"
  ```

* 修改配置文件

  *    /usr/local/etc/trojan/config.json
  *    @line 8: 设置一下密码
  *    @line 12: 设置证书密钥路径

  ```json
  {
      "run_type": "server",
      "local_addr": "0.0.0.0",
      "local_port": 443,
      "remote_addr": "127.0.0.1",
      "remote_port": 80,
      "password": [
          "你的密码"
      ],
      "log_level": 1,
      "ssl": {
          "cert": "/etc/letsencrypt/live/certificate.crt",
          "key": "/etc/letsencrypt/live/private.key",
          "key_password": "",
          "cipher": "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384",
          "cipher_tls13": "TLS_AES_128_GCM_SHA256:TLS_CHACHA20_POLY1305_SHA256:TLS_AES_256_GCM_SHA384",
          "prefer_server_cipher": true,
          "alpn": [
              "http/1.1"
          ],
          "alpn_port_override": {
              "h2": 81
          },
          "reuse_session": true,
          "session_ticket": false,
          "session_timeout": 600,
          "plain_http_response": "",
          "curves": "",
          "dhparam": ""
      },
      "tcp": {
          "prefer_ipv4": false,
          "no_delay": true,
          "keep_alive": true,
          "reuse_port": false,
          "fast_open": false,
          "fast_open_qlen": 20
      },
      "mysql": {
          "enabled": false,
          "server_addr": "127.0.0.1",
          "server_port": 3306,
          "database": "trojan",
          "username": "trojan",
          "password": "",
          "key": "",
          "cert": "",
          "ca": ""
      }
  }
  
  ```

* 启动trojan

  ```
  service trojan sart
  ```

* 更新证书

  acme.sh 每三个月自动更新证书后需要通知Tojan 重新加载证书。可以通过发送SIGUSER1 信号给 trojan 进程重新加载证书。使用 crontab，每个月通知一次 trojan 重新加载证书。

  ```
  crontab -e
  
  #在底下添加一行新任务
  0 0 1 * * killall -s SIGUSR1 trojan
  
  #查看crontatb 是否生效
  crontab -l
  ```

  

* 允许开机自动启动

  ```bash
  systemctl enable trojan
  systemctl enable nginx
  ```

  #### 开启 bbr

  bbr 是google 提出的一种新型拥塞控制算法，可以使linux 服务器显著提高吞吐量和减少TCP 连接的延迟。

  ```bash
  # 查看是否已开启
  sysctl net.ipv4.tcp_congestion_control
  # 开启 bbr
  sudo bash -c 'echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf'
  sudo bash -c 'echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf'
  sudo sysctl -p
  ```

  