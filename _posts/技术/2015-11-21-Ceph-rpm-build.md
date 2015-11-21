---
layout: post
title: Ceph-deploy、Ceph的RPM包制作
category: 技术
tags: Ceph
keywords: 
description: 
---
首先安装rpmbuild包，查看rpmbuild的工作目录：

~/rpmbuild

~/rpmbuild/SOURCES
 
~/rpmbuild/SPECS 

~/rpmbuild/BUILD
 
~/rpmbuild/RPMS
 
~/rpmbuild/SRPMS 

如果你的用户目录主目录下没有类似目录结构，你可以通过yum install rpmdevtools来自动配置和生成。

运行rpmdev-setuptree自动配置命令自动生成如上目录，并配置一些必要操作。

#Ceph-deploy rpm包

##1.下载源码
git clone https://github.com/ceph/ceph-deploy.git

easy_install virtualenv

##2.执行./bootstrap
会下载Vendor代码，gpg认证的几行注释掉，遇到问题安装依赖包

##3.编译
scripts/build-rpm.sh

#Ceph rpm包

##1.下载源码
wget -P ~/rpmbuild/SOURCES/ http://ceph.com/download/ceph-0.94.2.tar.bz2

##2. spec和patch文件
tar --strip-components=1 -C ~/rpmbuild/SPECS/ --no-anchored -xvjf ~/rpmbuild/SOURCES/ceph-0.94.2.tar.bz2 "ceph.spec"

在github的Ceph工程中下载rpm目录下的init-ceph.in-fedora.patch并拷贝到~/rpmbuild/SOURCES/

##3. build
rpmbuild -ba ~/rpmbuild/SPECS/ceph.spec

