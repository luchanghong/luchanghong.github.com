--- 
wordpress_id: 437
wordpress_url: http://luchanghong.com/rosemary/?p=437
date: 2012-07-23 11:22:01 +08:00
layout: post
title: switch your python env with using pythonbrew
category: python
tags: [python, pythonbrew]
description: 不同的项目可能用不同版本的 python 开发，而且装的 package 也不一样，为了便于管理，随时切换我们的 python 环境，推荐使用 pythonbrew 。
---
mac本上装的是python2.7.1，而项目开发用的是2.6版本的，虽然都是2.*，但是还是有些差别的，那么可不可以在系统里安装多个版本的python，用哪个版本就切换到哪个版本呢？pythonbrew帮我解决了这个问题。

## 一、pythonbrew的安装

```bash
pip install pythonbrew
```

或者

```bash
easy_install pythonbrew
```

然后在当前shell下执行

```bash
pythonbrew_install
```

接着修改~/.bash_profile

```ini
[[ -s $HOME/.pythonbrew/etc/bashrc ]] &amp;&amp; source $HOME/.pythonbrew/etc/bashrc
```

OK，这样就安装完毕了

## 二、使用pythonbrew

1.查看系统可以安装的python版本

```bash
pythonbrew list --know
```

2.安装一个python2.6.7

```bash
pythonbrew install 2.6.7
```

3.清理安装编译是的文件

```bash
pythonbrew cleanup
```

4.查看当前pythonbrew下的python版本有哪些（后面有*号表示正在使用）

```bash
pythonbrew list
```

5.选择一个python版本使用

```bash
pythonbrew use 2.6.7
```

6.使用python2.6.7作为系统python使用

```bash
pythonbrew switch 2.6.7
```

7.退出pythonbrew选择的python

```bash
pythonbrew off
```

<span style="color: #ff0000;">注意：只有switch之后才算是把系统python切换过来了，否则再次打开终端，键入python，版本还是原来的版本</span>

## 三、结合virtualenv使用

通常单独安装一个virtualenv来使用，现在通过pythonbrew来建立虚拟环境，前提条件是先选择一个python版本（pythonbrew use ***）。

1.创建虚拟环境

```bash
pythonbrew venv create my_env
```

2.虚拟环境列表

```bash
pythonbrew venv list
```

3.使用虚拟环境

```bash
pythonbrew venv use my_env</pre>
```

4.退出虚拟环境

```bash
deactivate
```

在使用pythonbrew的时候，都会有提示说明，比较容易看懂。
