--- 
wordpress_id: 297
wordpress_url: http://luchanghong.com/rosemary/?p=297
date: 2012-06-15 00:03:08 +08:00
layout: post
title: !binary |
  5q2j5YiZ566A6K6w5b2S57qz
category: python
tags: [python]
description: 网上的正则表达式的用法讲的很详细，但是不易读，我简单归纳了一下常用的一些字符和表达式。
---
一、单个字符匹配
<ul>
	<li>' . '  匹配任一个除换行‘\n'之外的字符</li>
	<li>' \ ' 转义字符</li>
	<li>' [...] ' 从中取一个</li>
</ul>
二、数量词的含义
<ul>
	<li>' * '  :  &gt;=0</li>
	<li>' + ' :  &gt;0</li>
	<li>' ? ' :  0/1</li>
	<li>{m}:  =m</li>
	<li>{m, n} :  [m, n]（区间表示）</li>
	<li>{m, } :  [m, +∞)</li>
	<li>{ , n} :  [0, n]</li>
</ul>
三、预定义字符（集合）
<ul>
	<li>' \d ' : 整数集合[0, 9]</li>
	<li>' \D ' : [^\d] （小d取反）</li>
	<li>' \s' : [\n\t\r\f]</li>
	<li>' \S ' : [^\s]</li>
	<li>' \w ' : [a-zA-Z0-9_] : 注意还包括下划线</li>
	<li>' \W ' : [^\w]</li>
</ul>
四、定界符
<p style="padding-left: 30px;">' ^ ' 开始  ' $ ' 结束</p>
