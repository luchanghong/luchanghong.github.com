--- 
wordpress_id: 526
wordpress_url: http://luchanghong.com/rosemary/?p=526
layout: post
title: !binary |
  5Yid6K+VZ2l0aHVi
category: web
tags: [github]
description: 第一次使用 github ，了解一下 git 和 github 的关系。
---
说实话，做这么久程序猿却没有真刀实枪的在github上混过真是有点小难堪，不过还好我早已经注册了账号。

网上搜索关于github的资料很多，一个一个看难免耽误时间。看了一个<a href="http://rogerdudler.github.com/git-guide/index.zh.html" target="_blank">官方介绍（中文版）</a>，然后就登陆账号捣鼓一下，有所收获。

<strong>方便之门：ssh</strong>

设置一个SSH key免去了每次输入账号、密码之苦，最近刚写一篇文章提到过<a title="mac下ssh自动登陆" href="http://luchanghong.com/rosemary/?p=513" target="_blank">如何用ssh自动登陆服务器</a>，那么这个也是同样的道理了，只不过没有使用shell的权利，github不提供，当你测试的时候就会发现（往下看）。

看<a href="https://help.github.com/articles/generating-ssh-keys" target="_blank">官方的帮助文档</a>，命令

<pre class="prettyprint">ssh-keygen -t rsa -C xxxxx</pre>刚开始以为-C参数必须使用github对应的账号，所以导致我登陆其他服务器的id_rsa不能和github的id_rsa同时使用，很郁闷，不过随后经过证实这个是自定义的，那么我就仍然用同一对key。

<a href="/upload/2012/08/B5FBBC28-77A8-4EE3-B2BD-66CE36E18305.jpg"><img class="alignnone size-full wp-image-527" title="B5FBBC28-77A8-4EE3-B2BD-66CE36E18305" src="/upload/2012/08/B5FBBC28-77A8-4EE3-B2BD-66CE36E18305.jpg" alt="" width="650" height="123" /></a>

前一个title是手动指定-C参数生成的，后一个是不写-C参数自动生成的。完了之后测试

<pre class="prettyprint">
lchmatoMacBook-Pro:.ssh lch$ ssh -T git@github.com
Hi luchanghong! You've successfully authenticated, but GitHub does not provide shell access.</pre>

<strong>安装git</strong>

git是个类似svn的版本控制管理工具，而github是一个使用git作为交流沟通工具的社区，我是这么理解的。下载git并安装之后，就可以和github上的项目打交道了，而上面设置的SSH key就是我们的通行证。

<strong>git的使用</strong>

git的使用方法有很多资料介绍和演示，慢慢练习吧。真心实意的clone几个开源项目来学习研究，和别人探讨交流，既增长git使用的熟练程度，又能提高自身编程，开开眼界，看看别人都在做什么，何乐而不为呢？
