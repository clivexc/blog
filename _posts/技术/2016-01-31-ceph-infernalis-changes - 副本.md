---
layout: post
title: Ceph Infernalis重要变化
category: 技术
tags: Ceph
keywords: 
description: 
---
Ceph Infernalis v9.2.0早已发布，我们也在自己的实验环境中使用过一段时间了，现将相对于Hammer版本的两大重要变化说明如下：

##1.以ceph用户启动进程
比如monitor进程启动参数中增加了--setuser和-setgroup两个参数：

	/usr/bin/ceph-mon -f --cluster ceph --id node1 --setuser ceph --setgroup ceph

所以在部署Ceph的准备阶段必须先创建Ceph用户，这也是为了安全性考虑：

	useradd -d /home/ceph -m ceph -s /bin/bash
	passwd ceph 

不过还是得给Ceph用户sudo权限：

	echo "ceph ALL =(root) NOPASSWD:ALL" | tee /etc/sudoers.d/ceph
	chmod 0440 /etc/sudoers.d/ceph 

部署过程中遇到问题，或者ceph集群访问不时，很可能是权限问题，需要将相应的文件或目录权限修改为ceph用户，比如：
   
	sudo chown ceph:ceph /etc/ceph/*
	sudo chown ceph:ceph /var/lib/ceph/*
	sudo chown ceph:ceph /var/run/ceph/*

当然，如果安装ceph时，本身是在ceph用户下，这些权限一般是没有问题的。

在Ubuntu上发现journal盘的权限也需要手动修改为Ceph用户的，才能部署成功，这个问题在Ceph-deploy的buglist里面也已经存在了，有待后续跟踪。

##2.进程启动方式变为systemd方式（部分Ubuntu版本除外）
如果使用Ceph-deploy部署，对应的版本至少为1.5.30，管理ceph以systemd方式启动的脚本在如下目录：

	[ceph@node1 system]$ pwd
	/usr/lib/systemd/system
	[ceph@node1 system]$ ll *ceph*
	-rw-r--r--. 1 root root 317 Nov 10 20:06 ceph-create-keys@.service
	-rw-r--r--. 1 root root 202 Nov 10 20:06 ceph-disk@.service
	-rw-r--r--. 1 root root 424 Nov 10 20:06 ceph-mds@.service
	-rw-r--r--. 1 root root 651 Nov 10 20:06 ceph-mon@.service
	-rw-r--r--. 1 root root 535 Nov 10 20:06 ceph-osd@.service
	-rw-r--r--. 1 root root 388 Nov 10 20:06 ceph-radosgw@.service
	-rw-r--r--. 1 root root 128 Nov 10 20:06 ceph.target

rhel7.1上systemd的版本升级到219以上才能使进程enable，systemd的常用命令将在下一篇文档中介绍。

另外，Ceph-deploy部署时，在每个进程工作目录中都有对应的systemd文件，比如：

	[ceph@node1 ceph-0]$ pwd
	/var/lib/ceph/osd/ceph-0
	[ceph@node1 ceph-0]$ ll
	total 44
	-rw-r--r--.   1 root root  481 Jan 29 10:44 activate.monmap
	-rw-r--r--.   1 ceph ceph    3 Jan 29 10:42 active
	-rw-r--r--.   1 ceph ceph   37 Jan 29 10:34 ceph_fsid
	drwxr-xr-x. 102 ceph ceph 1605 Jan 29 16:32 current
	-rw-r--r--.   1 ceph ceph   37 Jan 29 10:34 fsid
	lrwxrwxrwx.   1 ceph ceph   58 Jan 29 10:34 journal -> /dev/disk/by-partuuid/e63334ab-0e7c-46e9-8448-9586c45e3c57
	-rw-r--r--.   1 ceph ceph   37 Jan 29 10:34 journal_uuid
	-rw-------.   1 ceph ceph   56 Jan 29 10:44 keyring
	-rw-r--r--.   1 ceph ceph   21 Jan 29 10:34 magic
	-rw-r--r--.   1 ceph ceph    6 Jan 29 10:44 ready
	-rw-r--r--.   1 ceph ceph    4 Jan 29 10:34 store_version
	-rw-r--r--.   1 ceph ceph   53 Jan 29 10:34 superblock
	-rw-r--r--.   1 root root    0 Jan 31 10:38 systemd
	-rw-r--r--.   1 ceph ceph    2 Jan 29 10:34 whoami

