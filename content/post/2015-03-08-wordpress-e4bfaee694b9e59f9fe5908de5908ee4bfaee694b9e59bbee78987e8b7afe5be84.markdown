---
author: wuqt
date: 2015-03-08 15:08:52+00:00
draft: false
title: Wordpress 修改域名后修改图片路径
type: post
url: /
categories:
- Website Manage
---

[hermit auto="1" loop="0" unexpand="0"]netease_songs#:65538[/hermit]
WordPress更换域名 主页和图片路径错误 解决办法
首先介绍下SQL替换命令
**UPDATE 表名 SET 字段 = REPLACE(字段,’替换内容’,'替换值’);**
eg：
**UPDATE** wp_options** SET** option_value =** REPLACE**(option_value,'www.59a.cn','m59a.cn');
其中wp_options就是表名，option_value就是表wp_options里的一个字段，wp_options里有siteurl和home的值。
一般只要执行以下命令，就可完成域名的修改：
1.修改option_value里的站点url和主页地址：
UPDATE wp_options SET option_value = replace(option_value, 'http://www.59a.cn', 'http://www.new-59a.cn') WHERE option_name = 'home' OR option_name = 'siteurl';
2.更正文章中内部链接及附件的地址, 主要是一些图片路径：
UPDATE wp_posts SET post_content = replace(post_content, 'http://www.59a.cn', 'http://www.new-59a.cn');
3.更正wordpress文章默认的永久链接：

UPDATE wp_posts SET guid = replace(guid, 'http://www.59a.cn','http://www.new-59a.cn');
