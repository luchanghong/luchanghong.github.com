--- 
wordpress_id: 222
wordpress_url: http://luchanghong.com/rosemary/?p=222
date: 2012-05-17 18:51:12 +08:00
layout: post
title: !binary |
  RGphbmdv5byA5Y+R5a2m5Lmg77yI5LiJ77yJ
category: python
tags: [python, django]
description: 先来了解一下 django 的后台。后台管理其实就是一个 app ，我们只管安装配置，照着文档走一遍就大致了解了。
---
开始从后台说起，然后再转到前台。

一、开启Django后台

首先，在配置文件中解开INSTALLED_APPS的配置：
<pre class="prettyprint">
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls',
    # Uncomment the next line to enable the admin:
    'django.contrib.admin',
    # Uncomment the next line to enable admin documentation:
    # 'django.contrib.admindocs',
)
</pre>
然后，去mysite/urls.py解开admin URL的配置：
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
</pre>
最后，run command:

<pre class="prettyprint">python manage.py syncdb</pre>

此时，进入后台<a href="http://localhost:8000/admin/">http://localhost:8000/admin/</a>

<a href="/upload/2012/05/admin-logo.jpg"><img class="alignnone size-full wp-image-223" title="admin-logo" src="/upload/2012/05/admin-logo.jpg" alt="" width="592" height="385" /></a>

这里的后台用户名和密码，在第一次跑数据库（run上面的命令）的时候会有提示是否添加后台管理员帐户，如果没有添加，在数据库的auth_user数据表添加一条数据，is_active、is_superuser字段设为1即可，然后登录进去。

<a href="/upload/2012/05/admin-manage.jpg"><img class="alignnone size-full wp-image-224" title="admin-manage" src="/upload/2012/05/admin-manage.jpg" alt="" width="526" height="199" /></a>

二、在后台添加polls应用管理

还是以polls为例。在polls的目录下创建一个admin.py的文件，这样写：
<pre class="prettyprint">
#-*- coding: utf-8 -*-
__author__ = 'luchanghong'
from models import Poll,Choice
from django.contrib import admin

admin.site.register(Poll)
admin.site.register(Choice)
</pre>
然后停止服务，再启动，run command:

<pre class="prettyprint">python manage.py runserver</pre>

注意：项目文件改变的时候Django会自动检测到并重启，而新增加文件则不会，所以要重启一下server。

刷新一下页面，就会看到polls的两个数据模型对象被添加进来了：

<a href="/upload/2012/05/admin-polls.jpg"><img class="alignnone size-full wp-image-225" title="admin-polls" src="/upload/2012/05/admin-polls.jpg" alt="" width="526" height="273" /></a>

接下来就去点点看具体的功能吧，其实无非就是数据操作：增删改查。

三、admin.py详细设置

admin.py可以增加一些对数据对象的定义，比如：显示字段排序、搜索字段、字段分类等等，如下配置：
<pre class="prettyprint">
#-*- coding: utf-8 -*-
__author__ = 'luchanghong'
from models import Poll,Choice
from django.contrib import admin

class ChoiceInline(admin.StackedInline):
    model = Choice
    extra = 3

class PollAdmin(admin.ModelAdmin):
    #fields = ['pub_date', 'question']
    fieldsets = [
        ('BaseInfo', {'fields': ['question']}),
        ('ImportantInfo', {'fields': ['pub_date'], 'classes': ['collapse']})
    ]
    inlines = [ChoiceInline]
    list_display = ['question', 'pub_date', 'was_published_recently']
    list_filter = ['pub_date']
    search_fields = ['question']
    date_hierarchy = 'pub_date'

admin.site.register(Poll, PollAdmin)
admin.site.register(Choice)
</pre>
可以逐项的增加，看一下后台到底有什么变化，就会明白设置的作用了，其实看一下属性的英文意思就大致明白了。

注意：有些属性是不能共存的，比如fields和fieldsets。

我也是在学习之中，有什么不对的或者需要改进的欢迎交流讨论。不明白的去看手册或者google一下，师傅领进门，修行在自身啊！

接下来的学习就是给Django设置URL路径以及写VIEW，再渲染上模版，我想Django入门学习应该差不多了吧。
