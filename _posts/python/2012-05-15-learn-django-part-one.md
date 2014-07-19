--- 
wordpress_id: 196
wordpress_url: http://luchanghong.com/rosemary/?p=196
date: 2012-05-15 19:32:32 +08:00
layout: post
title: Django开发学习（一）
category: python
tags: [python, django]
description: 从我了解到的 pythoner 以及相关技术交流讨论组里面来看，国内用 python 做 WEB 开发用的框架中 django 算是比较火的了，所以抽空来学习一下 django 。
---
python的WEB框架，之前公司项目用到的是pyramid，在Q群里几乎没人用过，用Django的挺多的，于是抽空学一下Django。

我的开发（软件）环境：

OS：WIN7 32bit

Python：2.6  <a href=" http://www.python.org"> http://www.python.org</a>

Django：1.4   <a href="https://www.djangoproject.com/download/1.4/tarball/">https://www.djangoproject.com/download/1.4/tarball/</a>

一、下载并安装Django

去官网下载，上面给出地址。下载后解压并安装：python setup.py install，这里就会安装在python/lib/site_package下，作为一个类库。

二、创建一个project

在python/lib/site_package/django/bin下有一些命令，可以help看一下。创建一个project：
<pre>django-admin.py startproject mysite</pre>
<pre>注意：会在当前目录下创建一个mysite目录，这就是一个初始的Django框架。</pre>
<pre>三、Django，run</pre>
<pre>想到一部电影《chicken,run》，下面来run Django，进入mysite目录，命令：</pre>
<pre>python manage.py runserver</pre>
<pre>默认启用的是8000端口，当然你可以认为改变，只要不冲突，如下：</pre>
<pre><a href="/upload/2012/05/django.jpg"><img class="alignnone size-full wp-image-197" title="django" src="/upload/2012/05/django.jpg" alt="" width="681" height="136" /></a></pre>
<pre>OK，浏览器打开<a href="http://localhost:8080/">http://localhost:8080/</a>，It worked!</pre>
<pre><a href="/upload/2012/05/django2.jpg"><img class="alignnone size-full wp-image-198" title="django2" src="/upload/2012/05/django2.jpg" alt="" width="670" height="586" /></a></pre>
<pre>停止服务的话，可不是一般的CTRL+C，看他说明：Quit the server with CTRL-BREAK. break键在哪，找找吧！</pre>
<pre>四、连接数据库</pre>
<pre>连接数据库就要找到配置文件了，在mysite\mysite\settings.py里面，我配置的是mysql。</pre>
<pre>首先修改数据库连接等变量，在line 12，例如：</pre>

```python
DATABASES = {
    'default': {
        #'ENGINE': 'django.db.backends.',     # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
        'ENGINE': 'django.db.backends.mysql', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
        'NAME': 'django',                    # Or path to database file if using sqlite3.
        'USER': 'root',                      # Not used with sqlite3.
        'PASSWORD': 'root',                  # Not used with sqlite3.
        'HOST': 'localhost',                 # Set to empty string for localhost. Not used with sqlite3.
        'PORT': '3306',                      # Set to empty string for default. Not used with sqlite3.
    }
}
```

每次修改并保存配置文件之后，服务都会自动重启。配置完成之后，提示找不到MySQLdb这个类，那么就去装吧。
安装的过程比较头疼，这个好久没人维护了估计，出现一堆问题，推荐解决办法：<a href="http://i.19830102.com/archives/164">http://i.19830102.com/archives/164</a>
如果你用的不是python2.6版本，或者本地安装的mysql版本也和MySQLdb这个module有些冲突的话，就好好去google一下吧，会有解决办法的。
五、导入Django自带的app
连接好数据库之后，导入一下Django自带的app，配置文件已给出说明，如下：

```python
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Uncomment the next line to enable the admin:
    # 'django.contrib.admin',
    # Uncomment the next line to enable admin documentation:
    # 'django.contrib.admindocs',
)
```

<pre>然后，敲一下命令：python manage.py syncdb，就会在数据库建立一些相关的数据表：</pre>
<a href="/upload/2012/05/django3.jpg"><img class="alignnone size-full wp-image-199" title="django3" src="/upload/2012/05/django3.jpg" alt="" width="595" height="333" /></a>
今天就学到这了，其实都是看官网documentation跟着做的，顺便吐槽一下：学编程不要对英文排斥、反感。
