--- 
wordpress_id: 493
wordpress_url: http://luchanghong.com/rosemary/?p=493
date: 2012-08-13 15:13:10 +08:00
layout: post
title: mongo一般分析方法
category: database
tags: [mongo]
description: 某一天突然觉得 mongo 数据库变得很慢，那么改如何分析呢？ mongo 不仅自身提供了几个查看状态的工具（ mongotop、 mongostat ），我们还可以登陆到 mongo shell 里面一探究竟，通过这些操作应该能确定 mongo 的问题所在，然后在对症下药。
---
mongo很慢，突然！果断分析一下。

一、mongo自带工具
<ul>
	<li>mongotop
用法：<pre class="prettyprint">mongotop --host 192.168.1.111</pre> 主要是查看每秒读写排名靠前的几个数据集。</li>
	<li>mongostat
用法：<pre class="prettyprint">mongostat --host 192.168.1.111</pre>查看整个mongo的状态，主要看faults和miss：faults表示每秒错误数（linux下才有此统计项），如果大于100就要看一下服务器内存是否够用了；miss最好控制50%以下。另外看一下整体的I/O流量对比，还有查询的方法（query/insert/update/delete）的比例等等。</li>
</ul>
二、进入mongo的shell里面
<ul>
	<li>db.stats()，查看当前db的一些概况：数据集、大小、索引相关，ok=1表示状态OK了。</li>
	<li>db.serverStatus()，查看更加详细的信息：全局锁、容量、连接数、异常信息、索引信息等，这里可以看当前连接数和可用连接数，最好不要超过2000个连接，然后看一下索引的出错命中率（也就是没有命中）missRatio，目前我的是0.000142……</li>
	<li>db.currentOp()，查看当前执行的数据操作，每一次刷新可以看到当前正在执行的增删改查的具体命令，如果长时间没有结束（卡死，但是active仍是true）就手动杀掉，db.killOp(12123)，12123是要杀掉的opid</li>
</ul>
三、mongo辅助工具

这一类的工具我还没使用过，分析的结果无非也是通过执行这些命令分析之后得到的，只不过图形化更易懂，而且不用敲命令显得更方便。

说一下我遇到的问题吧，mongo过慢-》currentOp太多-》卡死的op过多-》mongostat表示faults过大-》killop

最终的问题是数据库太大，300G了，机器内存较小导致，目前优化也不行了~~~
