--- 
wordpress_id: 437
wordpress_url: http://luchanghong.com/rosemary/?p=437
date: 2012-07-23 11:22:01 +08:00
layout: post
title: !binary |
  cHl0aG9uYnJld+KAlOKAlOmaj+W/g+aJgOassueahOmAieaLqXB5dGhvbueJ
  iOacrDIuKi8zLio=
category: python
tags: [python, pythonbrew]
description: 不同的项目可能用不同版本的 python 开发，而且装的 package 也不一样，为了便于管理，随时切换我们的 python 环境，推荐使用 pythonbrew 。
---
mac本上装的是python2.7.1，而项目开发用的是2.6版本的，虽然都是2.*，但是还是有些差别的，那么可不可以在系统里安装多个版本的python，用哪个版本就切换到哪个版本呢？pythonbrew帮我解决了这个问题。

<strong>一、pythonbrew的安装</strong>

[code]pip install pythonbrew[/code]或者[code]easy_install pythonbrew[/code]

然后在当前shell下执行<pre class="prettyprint">pythonbrew_install</pre>

接着修改~/.bash_profile<pre class="prettyprint">[[ -s $HOME/.pythonbrew/etc/bashrc ]] &amp;&amp; source $HOME/.pythonbrew/etc/bashrc</pre>

OK，这样就安装完毕了

<strong>二、使用pythonbrew</strong>

1.查看系统可以安装的python版本<pre class="prettyprint">pythonbrew list --know</pre>

2.安装一个python2.6.7<pre class="prettyprint">pythonbrew install 2.6.7</pre>

3.清理安装编译是的文件<pre class="prettyprint">pythonbrew cleanup</pre>

4.查看当前pythonbrew下的python版本有哪些（后面有*号表示正在使用）<pre class="prettyprint">pythonbrew list</pre>

5.选择一个python版本使用<pre class="prettyprint">pythonbrew use 2.6.7</pre>

6.使用python2.6.7作为系统python使用<pre class="prettyprint">pythonbrew switch 2.6.7</pre>

7.退出pythonbrew选择的python<pre class="prettyprint">pythonbrew off</pre>

<span style="color: #ff0000;">注意：只有switch之后才算是把系统python切换过来了，否则再次打开终端，键入python，版本还是原来的版本</span>

<strong>三、结合virtualenv使用</strong>

通常单独安装一个virtualenv来使用，现在通过pythonbrew来建立虚拟环境，前提条件是先选择一个python版本（pythonbrew use ***）。

1.创建虚拟环境<pre class="prettyprint">pythonbrew venv create my_env</pre>

2.虚拟环境列表<pre class="prettyprint">pythonbrew venv list</pre>

3.使用虚拟环境<pre class="prettyprint">pythonbrew venv use my_env</pre>

4.退出虚拟环境<pre class="prettyprint">deactivate</pre>

在使用pythonbrew的时候，都会有提示说明，比较容易看懂。