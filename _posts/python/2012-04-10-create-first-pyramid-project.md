--- 
wordpress_id: 118
wordpress_url: http://luchanghong.com/rosemary/?p=118
date: 2012-04-10 00:16:11 +08:00
layout: post
title: !binary |
  Q3JlYXRlIHlvdXIgZmlyc3QgcHlyYW1pZCBwcm9qZWN04oCU5Yib5bu65LiA
  5LiqcHlyYW1pZOmhueebrg==
category: python
tags: [python, pyramid]
description: 简单的过一下创建一个 pyramid project 的步骤。发现使用这个框架的人很少，不过它却是我用 python 做 WEB 开发用到的第一个框架。
---
前面一篇文章说到了pyramid创建一个简单的application，可以是一个单一的文件。用pyramid framework做WEB开发的时候，我们通常创建一个project，也就是一个简单的框架。

1.安装virtualenv
<p style="text-indent: 2em;">这是一个虚拟环境，因为我们安装的package都装载python目录下的Lib/site-packages里，一个项目越大，安装的package也就越多，不同的项目用到的package也都装在这个目录下，这样就使python目录很复杂，所以用virtualenv分别为每一个项目创建各自的虚拟python环境，他们的路径也可以自己定义，这样就使得管理简单化。virtualenv可以用pip或者easy_install来安装。</p>

<div style="text-indent: 2em;">
<pre class="prettyprint">
D:\Python27&gt;pip install -U virtualenv
D:\Python27&gt;virtualenv -h

D:\Python27&gt;F:
F:\&gt;cd python
F:\python&gt;virtualenv --no-site-packages pyramidenv
The --no-site-packages flag is deprecated; it is now the default behavior.
New python executable in pyramidenv\Scripts\python.exe
Installing setuptools................done.
Installing pip...................done.

F:\python&gt;pyramidenv\Scripts\activate.bat
(pyramidenv) F:\python&gt;pip install pyramid
</pre>
</div>
<p style="text-indent: 2em;">上面用到的--no-site-packages这个命令可以在help里面看到用法和说明，执行pyramidenv\Scripts\activate.bat是为了激活这个虚拟环境，然后安装pyramid，这样pyramid就会倍安装在pyramidenv这个目录里F:\python\pyramidenv\Lib\site-packages。</p>
2.安装paster
<p style="padding-left: 30px;"><del>paster是我们用到的服务器，可以用pip install paster这个命令来安装。</del></p>
<p style="text-indent: 2em;"><span style="color: #ff0000;">这一步可以省略，在上一步的时候会自动安装paste这个包，</span><span style="text-indent: 2em;"><span style="color: #ff0000;">声明一下：不好意思，昨晚这儿写错了。</span>去官网看一下</span><a style="text-indent: 2em;" href="http://pypi.python.org/pypi/PasteScript">http://pypi.python.org/pypi/PasteScript</a><span style="text-indent: 2em;">。如果</span><span style="text-indent: 2em;">在上一步完成后，在pyramidenv/Script/目录下没有看到pserve.exe、pcreate.exe等文件，用pip install paster这个命令来安装。</span></p>
3.创建一个pyramid project
<p style="text-indent: 2em;">上一步安装paster完成后，我们来创建一个pyramid项目，比如在F:\python目录下，cmd命令：pcreate -s starter firstProject，运行之后，就会生成F:\python\firstproject这个项目了。</p>
<p style="text-indent: 2em;">接下来，怎样让firstProject跑起来呢？到F:\python\firstproject下，发现有个setup.py，cmd下执行命令：</p>
<p style="text-indent: 2em;">python setup.py develop</p>
<p style="text-indent: 2em;">表示我们以开发的模式来run这个项目，等待一会，成功之后，再执行以下命令：</p>

<div style="text-indent: 2em;">
<pre class="prettyprint">
(pyramidenv) F:\python\firstProject&gt;pserve development.ini
Starting server in PID 5604.
serving on http://0.0.0.0:6543
</pre>
</div>
<p style="text-indent: 2em;">最为激动人心的时候到了，成功了，我们在浏览器输入<a href="http://localhost:6543/">http://localhost:6543/</a>，Welcome to firstProject!</p>
&nbsp;
