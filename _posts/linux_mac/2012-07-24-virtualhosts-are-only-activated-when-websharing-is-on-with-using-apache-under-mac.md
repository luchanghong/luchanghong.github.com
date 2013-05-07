--- 
wordpress_id: 450
wordpress_url: http://luchanghong.com/rosemary/?p=450
date: 2012-07-24 16:23:14 +08:00
layout: post
title: mac下apache虚拟域名配置问题——virtualhosts are only activated when WEBSHARING is on
category: linux_OSX
tags: [apache, mac]
description: 买 MacBook 了很高兴，最近也是捣鼓着。在 Apache 配置本地虚拟域名的时候出现了一些问题，具体表现是：不打开本地 WEB 共享就不能启用虚拟域名，最后通读了 mac 上 Apache 的配置文件才明白是怎么回事。
---
在我的mac本上配置apache虚拟域名的时候，出现了如题目所说的问题。

<strong>在网上可以找到很多mac笔记本apache配置虚拟域名的方法，总结有以下几个步骤：</strong>

## 1、修改httpd.conf

把下面一行的注释解开

```ini
#Include /private/etc/apache2/extra/httpd-vhosts.conf
```

vi搜索Virtual hosts就可快速定位到这一行的上一行。

## 2、修改extra/httpd-vhosts.conf

添加一个虚拟域名配置


```ini
<VirtualHost *:80>
DocumentRoot "/Users/lch/Sites/phpmyadmin"
ServerName phpmyadmin.com
</VirtualHost>
```

## 3、修改extra/httpd-usrdir.conf

添加对应的文件权限

```
<Directory "/Users/lch/Sites/phpmyadmin">
Options Indexes FollowSymLinks
AllowOverride None
Order allow,deny
Allow from all
</Directory>
```

4、修改系统hosts文件 /etc/hosts

添加一行

```
127.0.0.1        phpmyadmin.com
```

5、重启apache

如果没有启动就直接start，要不然就restart

```bash
sudo apachectl restart
```

然后检查apche是否启动成功

```bash
ps -ef | grep httpd
```

<span style="color: #ff0000;">注意：以上操作都是在web共享关闭情况下进行的</span>

到此，在浏览器输入配置的域名却提示404 NOT FOUND

<a href="/upload/2012/07/QQ20120724-1.png"><img class="alignnone size-full wp-image-451" title="QQ20120724-1" src="/upload/2012/07/QQ20120724-1.png" alt="" width="735" height="229" /></a>

配置肯定是没有问题了，网上的教程也是这么说的，但是我把web共享打开就可以了，那么到底问题出在哪里？一直困扰我两天，终于忍不住了通读httpd.conf配置文件。结果发现了坑爹的地方，有图为证：

<a href="/upload/2012/07/QQ20120724-3.png"><img class="alignnone size-full wp-image-452" title="QQ20120724-3" src="/upload/2012/07/QQ20120724-3.png" alt="" width="570" height="478" /></a>

只有WEBSHARING_ON才行，直接注释掉这两行，重启apache，终于OK了。

现在想想有点不解的是，为什么那么多教程都没有提及这一点呢，难道我新买的mac，apache配置文件改了？
