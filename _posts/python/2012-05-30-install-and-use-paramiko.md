--- 
wordpress_id: 270
wordpress_url: http://luchanghong.com/rosemary/?p=270
date: 2012-05-30 17:05:10 +08:00
layout: post
title: !binary |
  cGFyYW1pa2/nmoTlronoo4XkuI7kvb/nlKg=
category: python
tags: [python, paramiko]
description: 如果你要监控服务器，用 paramiko 是一个人很不错的选择，它可以建立 SSH 连接，像在终端一样执行各自命令反馈服务器的状态。
---
前段时间学习写服务器监控脚本，用paramiko。

一、安装

环境：WIN 7  32bit  + python2.7.2

装paramiko之前要先安装pyCrypto，可以在网上搜索下载到，但是建议不要下载源码包，那样自己编译会很麻烦，我是在CSDN下载WIN下的编译好的可执行文件进行安装，我下载的是这个<a href="/upload/2012/05/pycrypto-2.3.win32-py2.7.rar">pycrypto-2.3.win32-py2.7</a>

pyCrypto安装好之后，再安装paramiko，这个可以用pip或者easy_install安装：

<pre class="prettyprint">pip install paramiko</pre>

二、使用

我的需求比较简单，就是通过使用paramiko的SSHClient来登录到服务器并且用exec_command()函数来执行Linux下的命令，然后分析执行后的结果。
<pre class="prettyprint">
# -*- coding: utf-8 -*-
import paramiko

hostname = '192.168.1.201'
port = 22
username = 'root'
password = 'root'

try:
    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    ssh.connect(hostname, port, username, password)
    stdin, stdout, stderr = ssh.exec_command('ls -l')
    print stdout.read()
except paramiko.SSHException, e:
    print e
</pre>
登录方式有好多种，看一下手册吧。下面是上传和下载文件的方法：
<pre class="prettyprint">
import paramiko
t = paramiko.Transport(('192.168.1.201', 22))
t.connect(username = 'root', password = 'root')
sftp = paramiko.SFTPClient.from_transport(t)

file = '/home/root/app.xls'
sftp.put('app.xls', file)
sftp.get(file, 'app.xls')
t.close()
</pre>
put()就是上传，get()是下载方法。

&nbsp;
