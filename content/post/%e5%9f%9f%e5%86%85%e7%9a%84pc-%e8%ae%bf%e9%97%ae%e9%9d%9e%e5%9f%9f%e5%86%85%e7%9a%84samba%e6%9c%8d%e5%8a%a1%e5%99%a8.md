---
title: '域内的PC 访问非域内的samba服务器'
date: Tue, 23 Apr 2019 01:22:22 +0000
draft: false
tags: ['未分类']
---

![](http://www.wuquantai.com/wp-content/uploads/2018/04/微信图片_20180413232134-1024x768.jpg)

加入某个域内的PC 在登录samba服务器的时候，默认会在USERNAME 前加上DOMAIN\\, 所以一直登录失败。登陆窗口上也无法取消加 DOMAIN\\。

解决方法：

控制面板->凭据管理器(Credential Manager) -> 手动添加Windows 凭据， 这样就可以用此用户账户正常登录了。