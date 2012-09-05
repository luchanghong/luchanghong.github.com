--- 
wordpress_id: 63
wordpress_url: http://luchanghong.com/rosemary/?p=63
date: 2012-03-31 14:16:58 +08:00
layout: post
title: !binary |
  cHl0aG9u5a+5bGlzdOeahOaOkuW6j+aWueazlQ==
category: python
tags: [python, sort]
description: python 中列表是有序的，有时候需要根据值的大小排序；甚至一个字典也需要排序转换成列表。
---
python中对一个list排序，一般有两种方法：list.sort()和sorted(list)，区别很明显，前者改变list本身，后者生成一个新的list。

参数说明：
<p style="padding-left: 30px;">cmp: 比较</p>

<ol>
	<li>简单排序
<pre class="prettyprint">&gt;&gt;&gt; a
[(1, 'a'), (2, 'b'), (3, 'c'), (0, 'x')]
&gt;&gt;&gt; a.sort(reverse = True)
&gt;&gt;&gt; a
[(3, 'c'), (2, 'b'), (1, 'a'), (0, 'x')]
&gt;&gt;&gt; print sorted(a)
[(0, 'x'), (1, 'a'), (2, 'b'), (3, 'c')]
</pre></li>
	<li>安照list元素里面的某个值排序
<pre class="prettyprint">
&gt;&gt;&gt; a
[(3, 'c'), (2, 'b'), (1, 'a'), (0, 'x')]
&gt;&gt;&gt; print sorted(a, key = lambda l:l[0])
[(0, 'x'), (1, 'a'), (2, 'b'), (3, 'c')]
&gt;&gt;&gt; print sorted(a, cmp = lambda x,y:cmp(x[0], y[0]))
[(0, 'x'), (1, 'a'), (2, 'b'), (3, 'c')]
</pre></li>
	<li>说明
sort和sorted用法几乎一样，cmp和key这两个参数用法有所不同，key比cmp的效率要高，个人觉得key也比较好用，另外key的用法相对灵活：
按主次键值排序：
<pre class="prettyprint">
&gt;&gt;&gt; a
[(3, 'c'), (2, 'b'), (1, 'a'), (0, 'x'), (4, 'x')]
&gt;&gt;&gt; print sorted(a, key = lambda l:(l[1],l[0]), reverse = True)
[(4, 'x'), (0, 'x'), (3, 'c'), (2, 'b'), (1, 'a')]
&gt;&gt;&gt; print sorted(a, key = lambda l:(l[0],l[1]), reverse = True)
[(4, 'x'), (3, 'c'), (2, 'b'), (1, 'a'), (0, 'x')]</pre></li>
</ol>
