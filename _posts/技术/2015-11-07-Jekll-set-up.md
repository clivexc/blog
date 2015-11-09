---
layout: post
title: Jekyll搭建总结
category: 技术
tags: Jekyll
keywords: 
description: 
---
WIN7系统下搭建了Jekyll环境，分享如下

##1.安装rubyinstaller和DevKit
[ ruby官网 ](http://rubyinstaller.org/downloads/)上下载并安装，安装ruby时勾选环境变量选项。

在DevKit目录下执行ruby dk.rb init

然后在config.yml末尾增加一行 - C:\Ruby200-x64

然后执行:

ruby dk.rb review

ruby dk.rb install

##2.安装jekyll
打开DevKit目录中的msys.bat，然后执行gem install jekyll

##3.安装 Pygments
Jekyll里默认的语法高亮插件是 Pygments，它需要安装 Python。

###(1)安装Python(以3.4版本为例)
[ Python官网 ](http://www.python.org/download/)上下载并安装

环境变量PATH末尾添加C:\Python34和C:\Python34\Scripts

python -v命令有显示则正常
 
easy_install --version有显示则正常

###(2)安装Pygments

easy_install Pygments

##4.使用jekyll
jekyll new myblog

cd myblog

将_config.yml文件highlighter的值设置为Pygments

执行jekyll serve

现在就可以访问 http://localhost:4000了

