---
title: 'Web 开发环境搭建'
date: Sun, 21 Jun 2015 03:37:16 +0000
draft: false
tags: ['Website Manage']
---

1.安装eclipse。 2. TOMCAT依赖JDK运行，必须安装JDK，光有JRE不行。 JDK的环境变量配置： （1）先配置  JAVA\_HOME = C:\\Program Files\\Java\\jdk1.8.0\_77; （2）然后 PATH 里添加 %JAVA\_HOME%\\bin;%JAVA\_HOME%\\jre\\bin;这两个目录，以便命令行里能直接寻到这里。 注意: 安装 JDK时会默认在外部安装一个JRE，这里TOMCAT是依赖JDK运行的，所以JRE的环境变量目录要用JDK文件夹里的那个JRE，不要配置成外部的JRE目录了。 运行java -version 不出现java版本的一般是这里没配置好。 （2）最后是 CLASSPATH = .;%JAVA\_HOME%\\lib;%JAVA\_HOME%\\lib\\tools.jar; 注意：这里有三个目录，第一个是.; //表示当前工作目录，别丢了。运行TOMCAT时一闪而过一般是这个环境变量没设置好。 3. 安装 tomcat ，直接解压即可。 添加环境变量： CATALINA\_HOME = D:\\WebDev\\apache-tomcat-6.0.45-windows-x64\\apache-tomcat-6.0.45; //是你的tomcat目录。 PATH 里添加：%CATALINA\_HOME%\\bin; 点 bin/startup.bat直接可运行tomcat。或者命令行里打startup即可，若不行则是TOMCAT的 PATH变量没设置好。 尝试访问 http://localhost:8080，出现示例页说明配置成功。可以关闭控制台。 平时有要调试的网页文件可以拷到 webapps/ 底下，浏览器输入 http://localhost:8080/foldername/ 即可访问。 3.安装mysql。 **eclipse 配置tomacat 路径方法.** Window->Preference->Server->Runtime Environments Add : 添加 Tomecat 路径，即可。