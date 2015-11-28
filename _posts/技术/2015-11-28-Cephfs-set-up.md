---
layout: post
title: Cephfs搭建总结
category: 技术
tags: Ceph
keywords: 
description: 
---
在已有Ceph集群(Monitor和OSD)的前提下，安装Mds并挂载文件系统客户端，过程分享如下：

##1.安装mds
ceph-deploy mds create {host-name}

##2.创建fs

ceph osd pool create cephfs_data \<pg_num\>

ceph osd pool create cephfs_metadata \<pg_num\>

ceph fs new \<fs_name\> \<metadata\> \<data\>

ceph fs ls查看下

##3.挂载

###内核方式:
rhel7.1支持直接支持

    mount -t ceph {host-name}:{port}:/ {path} -o name=admin,secret=AQC+BkBWnr2THhAAsYeoAdq1LMRVY96guGylBw==
    {host-name}是monitor地址而不是mds节点的地址
    {path}是本地路径

###ceph-fuse方式:
安装ceph-fuse: yum install ceph-fuse

挂载：ceph-fuse -m {host-name}:{port} {path}

挂载后df能看到：

     10.10.10.10:6789:/     29335552    73728  29261824   1% /root/test
     ceph-fuse              29335552    73728  29261824   1% /root/test1
