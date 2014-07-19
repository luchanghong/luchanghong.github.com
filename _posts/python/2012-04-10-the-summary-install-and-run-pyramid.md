--- 
wordpress_id: 125
wordpress_url: http://luchanghong.com/rosemary/?p=125
date: 2012-04-10 16:37:27 +08:00
layout: post
title: the summary — install and run pyramid
category: python
tags: [python, pyramid]
description: 总结一下 pyramid 的安装过程，有出入的地方希望得到大家的指正，一起学习交流。
---
由于当初半路出家，本来是做PHP的，也没有接触过python，至于pyramid更是第一次用，所以这次总结也让我更加的了解和认识了pyramid。因此也出现了一些错误，昨晚那个paster就说错了，下次还是得搞的一清二楚再说，实在抱歉。

不过，第一次安装难免会遇到这样那样的问题，还是耐心一些，如果出错了解决不了就推到重来，没有什么大不了。另外，要学会看官方手册，尽管都是英文的，一定要坚持看下去，时间久了就习惯了。

在安装相关的package的时候，由于是在线安装的，所以网速不给力的话会比较慢，说不定还会卡死，我中午就遇到这样的问题（见下面截图）。等不及就先去官网下载，不过还是pip或者easy_install比较好，装的package比较全，我们自己去官网找可能有点困难。

<a href="/upload/2012/04/timeout.jpg"><img class="alignnone size-full wp-image-127" title="timeout" src="/upload/2012/04/timeout.jpg" alt="" width="681" height="440" /></a>

如果觉得虚拟环境比较别扭，那就直接装pyramid，然后创建一个project，接下来以develop的方式来跑一下setup.py，然后通过pserve启动项目。
<ul>
	<li>activate.bat进入虚拟python环境后，浏览script文件夹你会发现一个deactivate.bat这个命令，显然是退出虚拟环境的</li>
	<li>关于pcreate，可以-h一下看看参数说明和用法，-s的参数：starter/alchemy/zodb这三个值是pyramid包含的三种关于URL匹配和调度的架构。</li>
	<li>用到的一些命令，最好先-h一下，看一看有哪些用法、怎么用的、参数是什么……</li>
	<li>如果同事运行多个project，那么就修改development.ini里面的配置，把默认端口6543修改一下，否则相同端口只能启一个。host缺省值0.0.0.0也就是127.0.0.1也就是localhost。</li>
	<li>pserve可以看作paster serve，如：paster serve development.ini；另外可以加一个参数--reload，当.py文件改变的时候，会自动重启服务，否则不会重启也就看不到修改效果。</li>
</ul>
总结：多看文档，多操作，并且耐心。

<a href="/upload/2012/04/pyramid.jpg"><img class="alignnone size-full wp-image-128" title="pyramid" src="/upload/2012/04/pyramid.jpg" alt="" width="670" height="586" /></a>
