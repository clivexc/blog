---
layout: post
title: Jekyll搭建问题及解决
category: 技术
tags: Jekyll
keywords: 
description: 
---

Jekyll环境搭好后遇到的问题和解决方法，分享如下

##1.缺少插件包

安装完成后可能需要一些外部包，比方说jekyll-paginate，缺什么包就用gem install命令下载什么包，如果下载不了，可以删除ruby自带源，增加淘宝gem源：gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/

然后用下面命令查看一下是否生效：gem sources -l


##2. 本地编译正常，但是在github上build失败

错误信息： You have an error on line 1 of your `_config.yml` file

找了好久原来是文件编码格式问题，改成utf-8就可以了

##3. 路径解析问题

以我的第一篇文章为例，刚开始解析出来的链接是如下形式：http://clivexc.github.io/2015/11/07/Jekll-set-up.html_config.yml，缺少我的工程名blog。

解决方法：设置baseurl为/blog，blog就是github中的工程名，相应的检查所有路径中是否包含site.baseurl。

本地调试时改为jekyll serve --baseurl，也就是把baseurl参数覆盖为空 

看别人的资料后续改用域名后设置baseurl为／即可

