--- 
wordpress_id: 345
wordpress_url: http://luchanghong.com/rosemary/?p=345
date: 2012-07-05 17:22:52 +08:00
layout: post
title: !binary |
  dXJsbGliMuS9v+eUqOS4gOS6jOS4iQ==
category: python
tags: [python, urllib2]
description: 最近项目合作做些 API 之类的，用到 urllib2 这个 package ，简单来学习一下。
---
关于urllib2这个库就不用多说了，想必用到的人至少了解一番，这也是必须的。

<strong>使用目的：</strong>

可以理解模拟浏览器操作，一般是API接口调用。

<strong>使用方法：</strong>

1.最简单的

此种方法是用过urlopen()函数直接请求一个url，得到response。
<pre class="prettyprint">
# -*- coding: utf-8 -*-
__author__ = 'luchanghong'
from urllib2 import Request,urlopen,URLError

url = 'http://www.luchanghong.com'
try:
    content = urlopen(url)
    print content.read()
except URLError,e:
    print e
</pre>
2.Request请求

如果把url换成 http://blog.csdn.net ，你会发现出现403 error，因为csdn做了限制，拒绝了没有header头的请求，所以我们就加上header，这里用到Request
<pre class="prettyprint">
# -*- coding: utf-8 -*-
__author__ = 'luchanghong'
from urllib2 import Request,urlopen,URLError

# url = 'http://www.luchanghong.com'
url = 'http://blog.csdn.net'
try:
    request = Request(url)
    request.add_header('User-Agent', 'fake-client')
    content = urlopen(request)
    print content.read()
except URLError,e:
    print e
</pre>
header里面有好多内容，可以去百度百科查看一下。

3.传递参数

传递参数无非两种方法：GET、POST。GET就不用说了，直接在url后面加上参数就可以了。POST传参方法：先构建一个字典，然后通过urlencode()转换，也可以直接传递一个字符串（输出urlencode()的形式）。
<pre class="prettyprint">
import urllib,urllib2

url = 'http://www.luchanghong.com'
post = dict(
    user = 'luchanghong',
    password = '123456',
    token = 'aaabbbccc',
)
data = urllib.urlencode(post)
request = urllib2.Request(url, data)
</pre>
也可以用<pre class="prettyprint">request.add_data(data)</pre>直接用<pre class="prettyprint">urlopen(url = 'www.luchanghong.com', data = data)</pre>也是可以的

<strong>注意的问题：</strong>

NOTICE：建议做API调用还是用Request较好，因为设置及平台交互，有编码问题，还有Request的content-type的类型问题，而Request的header可以解决这些问题。

我今天就遇到content-type的问题：要POST一个json字符串过去，一定要设置header的context-type为application/json才行<pre class="prettyprint">request.add_header('Context-Type', 'application/json')</pre>一直以为API借口出问题了，因此郁闷好久。

<strong> 其他：</strong>

在解决问题的时候，我尝试了httplib2这个库，貌似比urllib2简单，而且不用设置header的content-type：
<pre class="prettyprint">
import httplib2,json

post = dict(
    user = 'luchanghong',
    password = '123456',
    token = 'aaabbbccc',
)
data = json.dumps(post)
conn = httplib2.Http()
response, content = conn.request(self.url,'POST', body = data)
print content
</pre>