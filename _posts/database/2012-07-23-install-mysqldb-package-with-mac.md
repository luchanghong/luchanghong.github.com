--- 
wordpress_id: 445
wordpress_url: http://luchanghong.com/rosemary/?p=445
date: 2012-07-23 13:52:29 +08:00
layout: post
title: Mac下安装MySQLdb
category: database
tags: [mac, MySQLdb, mysql]
description: MySQLdb 的安装要通过编译才行，而且前提是系统安装了 mysql ，在安装过程中还要进行一下配置。
---
换了笔记本是挺爽，但是好多东西都得重装。MySQLdb这个包在WIN下面就不是很好装，不过有编译好的exe安装文件，那么来学习在Mac下怎么安装的。

## 一、安装mySQL

安装这个包的前提是系统安装了mySQL，去mySQL官网下载符合系统版本的mySQL安装包，直接下载.dmg文件，打开之后双击安装，这个过程还是很简单的，也不用做什么限制。

## 二、安装MySQLdb

1.下载MySQLdb

下载地址：<a href="http://sourceforge.net/projects/mysql-python/">http://sourceforge.net/projects/mysql-python/</a>

2.解压并配置

解压下载的tar包，修改site.cfg配置文件：

```ini
#mysql_config = /usr/local/bin/mysql_config
```

把注释符号#去掉，后面的路径就是mysql_config的路径，在我的mac上是/usr/local/mysql/bin/mysql_config

3.build and install

```bash
python setup.py build
python setup.py install
```

&nbsp;
