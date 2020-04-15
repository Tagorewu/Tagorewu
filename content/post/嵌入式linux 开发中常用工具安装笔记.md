---
title: '嵌入式linux 开发中常用工具安装笔记'
date: Wed, 22 Aug 2018 11:25:05 +0000
draft: false
categories:
  - "Embedded"
tags: ['nfs', 'samba', '未分类']
---

  

*   Samba server    

> ```
> \# apt install samba samba-common
> # vim /etc/samba/smb.conf 添加：
> security = user
> \[shuji\]
>   comment = share dir
>   path = /srv/samba
>   browseable = yes
>   writable = yes
> # useradd public (不要用adduser, 否则还要禁用shell）
> # smbpasswd –a public # service smbd restart
> ```

*   Samba client

> ```
> smbclient -L //192.168.1.10 -U dev
> smbclient //192.168.1.10 -U dev
> ```

   

* * *

  

*   nfs

> ```
> \# vim /etc/exports
> ``````
> /srv/nfs \*(rw,sync,nosubtree\_check)
> ``````
> \# 查看nfs 
> # showmount -e 192.168.1.10
> # mount -t nfs 192.168.1.10:/srv/nfs4 /mnt/nfs
> ```