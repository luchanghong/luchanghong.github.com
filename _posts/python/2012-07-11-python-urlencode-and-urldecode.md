--- 
wordpress_id: 375
wordpress_url: http://luchanghong.com/rosemary/?p=375
date: 2012-07-11 15:06:58 +08:00
layout: post
title: "python\xE4\xB8\xAD\xE7\x9A\x84urlencode\xE4\xB8\x8Eurldecode"
category: python
tags: [python, urlencode, urldecode]
description: python 通过 HTTP 交互处理数据的时候，url 里面的中文以及特殊字符要做处理的，来学习一下 urlencode 与 urldecode 之间相互转换的方法。
---
当url地址含有中文，或者参数有中文的时候，这个算是很正常了，但是把这样的url作为参数传递的时候（最常见的callback），需要把一些中文甚至'/'做一下编码转换。

一、urlencode

urllib库里面有个urlencode函数，可以把key-value这样的键值对转换成我们想要的格式，返回的是a=1&amp;b=2这样的字符串，比如：
<pre class="prettyprint">
&gt;&gt;&gt; from urllib import urlencode
&gt;&gt;&gt; data = {
...     'a': 'test',
...     'name': '魔兽'
... }
&gt;&gt;&gt; print urlencode(data)
a=test&amp;name=%C4%A7%CA%DE
</pre>
如果只想对一个字符串进行urlencode转换，怎么办？urllib提供另外一个函数：quote()
<pre class="prettyprint">
&gt;&gt;&gt; from urllib import quote
&gt;&gt;&gt; quote('魔兽')
'%C4%A7%CA%DE'
</pre>

二、urldecode

当urlencode之后的字符串传递过来之后，接受完毕就要解码了——urldecode。urllib提供了unquote()这个函数，可没有urldecode()！

<pre class="prettyprint">
&gt;&gt;&gt; from urllib import unquote
&gt;&gt;&gt; unquote('%C4%A7%CA%DE')
'\xc4\xa7\xca\xde'
&gt;&gt;&gt; print unquote('%C4%A7%CA%DE')
魔兽
</pre>

三、讨论

在做urldecode的时候，看unquote()这个函数的输出，是对应中文在gbk下的编码，在对比一下quote()的结果不难发现，所谓的urlencode就是把字符串转车gbk编码，然后把\x替换成%。如果你的终端是utf8编码的，那么要把结果再转成utf8输出，否则就乱码。

可以根据实际情况，自定义或者重写urlencode()、urldecode()等函数。