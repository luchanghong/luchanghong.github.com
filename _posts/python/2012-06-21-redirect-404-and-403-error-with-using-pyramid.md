--- 
wordpress_id: 326
wordpress_url: http://luchanghong.com/rosemary/?p=326
date: 2012-06-21 09:06:03 +08:00
layout: post
title: !binary |
  5ZyocHlyYW1pZOS4remHjeWumuS5iTQwM+OAgTQwNOmUmeivrw==
category: python
tags: [python, pyramid]
description: 为了让网站和用户的交互更加友好，要定义一些 403 、404 错误页面，那么 pyramid 是怎样处理这些错误的呢？研究一下吧。
---
pyramid默认有403、404等出错处理机制，在pyramid.httpexceptions中可以看到。如果我们想让WEB对用户更加友好，那么就重定义一下这些错误，举例403 forbidden、404 not found。

在初始化myproject/__init__.py中：

方法一、
<pre class="prettyprint">
from pyramid.config import Configurator
config = Configurator()
config.add_forbidden_view(forbidden, renderer = '/templates/403.html')
def forbidden(request):
    return {}
</pre>
<pre>当然还有一个add_notfound_view。</pre>
方法二、

<pre class="prettyprint">
<pre>from pyramid.config import Configurator
from pyramid.httpexceptions import HTTPForbidden
config = Configurator()
config.add_view(forbidden, context = HTTPForbidden, renderer = '/templates/403.html')
def forbidden(request):
    return {}
</pre>
这里定义一个普通的VIEW，设置context参数为HTTPForbidden，同理也有HTTPNotFound

如果不定义renderer，那么要返回一个Response对象才行。
