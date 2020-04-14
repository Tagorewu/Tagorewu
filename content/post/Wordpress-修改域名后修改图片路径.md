---
title: 'Wordpress 修改域名后修改图片路径'
date: Sun, 08 Mar 2015 15:08:52 +0000
draft: false
tags: ['Website Manage']
---

\[hermit auto="1" loop="0" unexpand="0"\]netease\_songs#:65538\[/hermit\] WordPress更换域名 主页和图片路径错误 解决办法 首先介绍下SQL替换命令 **UPDATE 表名 SET 字段 = REPLACE(字段,’替换内容’,'替换值’);** eg： **UPDATE** wp\_options **SET** option\_value = **REPLACE**(option\_value,'www.59a.cn','m59a.cn'); 其中wp\_options就是表名，option\_value就是表wp\_options里的一个字段，wp\_options里有siteurl和home的值。 一般只要执行以下命令，就可完成域名的修改： 1.修改option\_value里的站点url和主页地址： UPDATE wp\_options SET option\_value = replace(option\_value, 'http://www.59a.cn', 'http://www.new-59a.cn') WHERE option\_name = 'home' OR option\_name = 'siteurl'; 2.更正文章中内部链接及附件的地址, 主要是一些图片路径： UPDATE wp\_posts SET post\_content = replace(post\_content, 'http://www.59a.cn', 'http://www.new-59a.cn'); 3.更正wordpress文章默认的永久链接： UPDATE wp\_posts SET guid = replace(guid, 'http://www.59a.cn','http://www.new-59a.cn');