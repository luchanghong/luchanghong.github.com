--- 
wordpress_id: 52
wordpress_url: http://luchanghong.com/rosemary/?p=52
date: 2012-03-27 14:23:22 +08:00
layout: post
title: !binary |
  bW9uZ29kYuWvvOWFpeWvvOWHuuaVsOaNrg==
category: database
tags: [mongodb]
description: 学习一下简单的 mongodb 数据导入导出方法，如果数据很庞大的话就寻求其他办法了。
---
使用mongodb，要把数据导出备份，用到mongodb自带的导入和导出工具。WIN7上操作如下：
<ol>
	<li>启动mongodb
这个就不用多说了~~~</li>
	<li>导出数据
进入CMD命令行模式：
<pre class="prettyprint">
D:\mongodb\bin&gt;mongoexport.exe -d adview -c user -o user.dat
connected to: 127.0.0.1
exported 4748 records
D:\mongodb\bin&gt;
</pre></li>
	<li>导入数据
<pre class="prettyprint">
D:\mongodb\bin&gt;mongoimport.exe -d test -c user user.dat
connected to: 127.0.0.1
imported 4748 objects
D:\mongodb\bin&gt;mongo.exe
MongoDB shell version: 2.0.1
connecting to: test
&gt; show collections
system.indexes
user
&gt; db.user.count()
4748
</pre></li>
	<li>mongo还可以导成CSV，包保存文件后缀改成.csv就可以了，有兴趣可以尝试一下</li>
</ol>
PS：-d 表示数据库(database)，-c表示数据集(collection)，如果想在本地的shell操作其他主机上的mongodb，可以在命令后面加上--host=1.1.1.1(主机IP地址)
