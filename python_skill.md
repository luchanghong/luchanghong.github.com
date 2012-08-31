---
date: 2012-08-27 17:35:54 +08:00
title: python小技巧集锦
layout: post
---
Python小技巧收集——简单实用：你会越来越爱Python

1.list去重

<pre class="prettyprint">
    {}.fromkeys(my_list).keys()
</pre>
2.保留2位小数

<pre class="prettyprint">
    print '%2.f' % my_float_number
</pre>
3.mySQL查询语句：select……in ()的时候把list里面字符串加引号输出

这种写法查询不出结果，除非对应字段类型是INT：

<pre class="prettyprint">
    ','.join(my_list)
</pre>
so，这样写：

<pre class="prettyprint">
    ','.join(map(lambda x: """'%s'""" % x , my_list))
</pre>
4.获取当前文件所在绝对路径

<pre class="prettyprint">
    import os
    print os.path.normpath(os.path.realpath(os.path.dirname(__file__)))
</pre>
