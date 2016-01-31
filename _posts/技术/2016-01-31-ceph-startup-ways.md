---
layout: post
title: Ceph启动方式对比
category: 技术
tags: Ceph
keywords: 
description: 
---

目前Ceph的不同版本，或者在不同操作系统上有如下三种启动方式：

##1.sysvinit:  

适用：Rhel/Centos (Infernalis以前的版本)

	service ceph [start|stop|restart] -a     
	service ceph [start|stop|restart] mon.node0
	service ceph [start|stop|restart] osd.0

##2.upstart:  

适用：Ubuntu

	[start|stop|restart] ceph-all
	[start|stop|restart] ceph-mon id=node0
	[start|stop|restart] ceph-osd id=0

##3.systemd: 

适用：Rhel/Centos (Infernalis及以后版本)

	systemctl [start|stop|restart] ceph.target
	systemctl [start|stop|restart] ceph-mon@node0      
	systemctl [start|stop|restart] ceph-osd@0

systmed是一个用户空间的程序，用户空间的第一个进程就是初始化进程，这个程序一般是/sbin/init，当然也可以通过传递内核参数来让内核启动指定的程序。这个进程的特点是进程号为1，代表第一个运行的用户空间进程。


