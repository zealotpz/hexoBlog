---
title: 'CentOS 7/6 安装 Subversion (SVN) 1.9 '
date: 2016-09-16 14:50:51
tags: svn
---

1. ** 配置 yum 源 **
安装前需先配置yum库,创建一个新的文件`/etc/yum.repos.d/wandisco-svn.repo`,内容如下:
```bash
[WandiscoSVN]
name=Wandisco SVN Repo
baseurl=http://opensource.wandisco.com/centos/$releasever/svn-1.9/RPMS/$basearch/
enabled=1
gpgcheck=0
```
<!--more-->
2. ** 安装 Subversion (SVN) 1.9 **
  安装新版本软件包之前先移除旧版本 svn
```bash
# yum remove subversion*
```
  用 yum 安装最新的Subversion包
```bash
# yum clean all
# yum install subversion
```

3. ** 验证svn是否安装成功 **
```bash
[svn@xxx ~]# svn --version
svn, version 1.9.5 (r1770682)
   compiled Dec  1 2016, 13:25:01 on x86_64-redhat-linux-gnu

Copyright (C) 2016 The Apache Software Foundation.
This software consists of contributions made by many people;
see the NOTICE file for more information.
Subversion is open source software, see http://subversion.apache.org/

The following repository access (RA) modules are available:

* ra_svn : Module for accessing a repository using the svn network protocol.
  - with Cyrus SASL authentication
  - handles 'svn' scheme
* ra_local : Module for accessing a repository on local disk.
  - handles 'file' scheme
* ra_serf : Module for accessing a repository via WebDAV protocol using serf.
  - using serf 1.3.7 (compiled with 1.3.7)
  - handles 'http' scheme
  - handles 'https' scheme

The following authentication credential caches are available:

* Plaintext cache in /root/.subversion
* Gnome Keyring
* GPG-Agent
```
显示`version 1.9.5`说明安装成功

** References: **
[http://tecadmin.net/install-svn-1-9-on-centos/#](http://tecadmin.net/install-svn-1-9-on-centos/#)
