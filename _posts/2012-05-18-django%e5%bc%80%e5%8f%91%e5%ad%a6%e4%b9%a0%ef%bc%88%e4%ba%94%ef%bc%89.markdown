--- 
wordpress_id: 244
wordpress_url: http://luchanghong.com/rosemary/?p=244
date: 2012-05-18 17:41:43 +08:00
layout: post
title: !binary |
  RGphbmdv5byA5Y+R5a2m5Lmg77yI5LqU77yJ

---
这篇文章主要是探讨一下URLconf到底怎样写以及出错页面（404、500）。

一、最原始的写法
<pre class="prettyprint">
from django.conf.urls import patterns, include, url

# Uncomment the next two lines to enable the admin:
from django.contrib import admin
admin.autodiscover()

urlpatterns = patterns('',
    # Examples:
    # url(r'^$', 'mysite.views.home', name='home'),
    # url(r'^mysite/', include('mysite.foo.urls')),
    # Uncomment the admin/doc line below to enable admin documentation:
    # url(r'^admin/doc/', include('django.contrib.admindocs.urls')),

    # Uncomment the next line to enable the admin:
    url(r'^admin/', include(admin.site.urls)),

    # polls
    url(r'^polls/$', 'polls.views.index'),
    url(r'^polls/(?P\d+)/$', 'polls.views.detail')
)
</pre>
为了保持和初始化状态保持一致，所以注释代码都没有删除。看一下polls这个app的URLconfig，每个VIEW方法都有“polls.views”这个前缀，可以单独提取出来，这样也有利于app的分类管理，如果当前project的app较多的话，例如：
<pre class="prettyprint">
from django.conf.urls import patterns, include, url

# Uncomment the next two lines to enable the admin:
from django.contrib import admin
admin.autodiscover()

urlpatterns = patterns('',
    # Examples:
    # url(r'^$', 'mysite.views.home', name='home'),
    # url(r'^mysite/', include('mysite.foo.urls')),
    # Uncomment the admin/doc line below to enable admin documentation:
    # url(r'^admin/doc/', include('django.contrib.admindocs.urls')),
    # Uncomment the next line to enable the admin:
    url(r'^admin/', include(admin.site.urls)),
)

urlpatterns += patterns('polls.views',
    url(r'^polls/$', 'index'),
    url(r'^polls/(?P\d+)/$', 'detail')
)
</pre>
这种写法看起来更简洁一些，就像是小学学习的加法分配率一样，而且把不同app的URLconfig分开来管理更方便些。

二、URLconf单独提出来

但是，当初说道project和app的关系的时候，app可以被多个project复用，就像是一个插件一样，如果每次复用的时候都要在project的主目录（暂且这么称呼）的urls.py里面写这些配置难免有些麻烦，所以能不能尽可能的把app独立出来就看URLconfig这块了，那么就把URLconf提到单独的app下面。

在colls文件夹下面建立urls.py：
<pre class="prettyprint">
#-*- coding: utf-8 -*-
__author__ = 'luchanghong'
from django.conf.urls import url, patterns, include

urlpatterns = patterns('polls.views',
    url(r'^$', 'index'),
    url(r'^(?P\d+)/$', 'detail')
)
</pre>
这时，把mysite/urls.py里面关于polls的URLconf删掉或者注释掉，然后加上一句：

<pre class="prettyprint">url(r'^polls/', include('polls.urls'))</pre>

三、出错页面

404是访问途径出错，找不到相应的页面，500是服务器内部错误。如果想启用这些出错页面，那么就在配置文件settings.py里把DEBUG设置为False。就以首页为例说明吧，默认的mysite/urls.py把首页的URL注释了：

<pre class="prettyprint"># url(r'^$', 'mysite.views.home', name='home')</pre>

那么，先不解开注释，直接访问，前提是把DEBUG关了，<a href="http://localhost:8000/">http://localhost:8000/</a>：

<a href="/upload/2012/05/404-1.jpg"><img class="alignnone size-full wp-image-247" title="404-1" src="/upload/2012/05/404-1.jpg" alt="" width="459" height="172" /></a>

接着，在mysite/templates/下建立404.html，随便写点东西，解开注释，在mysite/views.py里面写VIEW：
<pre class="prettyprint">
#-*- coding: utf-8 -*-
__author__ = 'luchanghong'
from django.http import Http404
from django.http import HttpResponse
from django.shortcuts import render_to_response

def home(request):
    raise Http404
</pre>
<a href="/upload/2012/05/404page.jpg"><img class="alignnone size-full wp-image-248" title="404page" src="/upload/2012/05/404page.jpg" alt="" width="459" height="172" /></a>

怎样出个500 server error呢？还是先建立一个500页面，然后把400.html删掉，当raise一个404但是却找不到404.html的时候就会抛出500.html了。

类似的，app也有他自己的404/500页面，当然建立在他自己的模板目录下。

<a href="/upload/2012/05/files.jpg"><img class="alignnone size-full wp-image-249" title="files" src="/upload/2012/05/files.jpg" alt="" width="191" height="485" /></a>
