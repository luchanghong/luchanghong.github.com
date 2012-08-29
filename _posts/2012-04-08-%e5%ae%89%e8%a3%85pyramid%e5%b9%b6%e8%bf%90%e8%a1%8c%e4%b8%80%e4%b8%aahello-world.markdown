--- 
wordpress_url: http://luchanghong.com/rosemary/?p=93
wordpress_id: 93
title: !binary |
  5a6J6KOFcHlyYW1pZOW5tui/kOihjOS4gOS4qmhlbGxvIHdvcmxk

layout: post
date: 2012-04-08 13:47:02 +08:00
---
公司的项目正式上线了，关于python的学习和pyramid框架的学习和应用也相对熟悉一些了，那么就趁热打铁把这段开发经历总结一下，以飨pyramid学习的朋友。

开发环境：WIN7  32bit

开发软件：python 2.7 + pyramid 1.3

1.安装python
<p style="padding-left: 30px;">在<a href="http://www.python.org">http://www.python.org</a>下载WIN 32bit 下的python2.7，然后安装。然后配置一下环境变量，如下图：</p>
<p style="padding-left: 30px;"><a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/04/setenv.jpg"><img class="alignnone  wp-image-97" title="setenv" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/04/setenv.jpg" alt="" width="698" height="491" /></a></p>
<p style="padding-left: 30px;">编辑的时候添加python的安装目录并以分号结尾，如：<span style="text-decoration: underline;">d:\python27;</span></p>
2.安装setuptools/pip
<p style="padding-left: 30px;">setuptools和pip是一个很好用的python包，用来安装或更新python其他的package。</p>
<p style="padding-left: 30px;">打开：<a href="http://pypi.python.org/pypi/setuptools">http://pypi.python.org/pypi/setuptools</a></p>
<p style="padding-left: 30px;">1）安装setuptools</p>
<p style="padding-left: 60px;">a.下载ez_setup.py</p>
<p style="padding-left: 60px;">这个文件可以在上面那网址找到下载，把ez_setup.py拷贝到python目录下。打开CMD，在python安装目录下执行：python ez_setup.py setuptools</p>

<div style="padding-left: 60px;">
<pre>[shell]
D:\&gt;cd Python27
D:\Python27&gt;python ez_setup.py setuptools
[/shell]</pre>
</div>
<p style="padding-left: 60px;">b.直接下载setuptools.exe</p>
<p style="padding-left: 60px;"><a href="http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11.win32-py2.7.exe" target="_blank">http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11.win32-py2.7.exe</a></p>
<p style="padding-left: 60px;">完成后配置环境变量 d:\python27\Scripts，这样就可以直接在cmd执行easy_install曾格格命令了。</p>
<p style="padding-left: 30px;">2）安装pip</p>
<p style="padding-left: 60px;"> 可以借助上面的ez_setup.py和easy_install来安装，如：</p>
<p style="padding-left: 60px;">easy_install -U pip</p>
<p style="padding-left: 60px;">安装了pip之后，就可以使用pip了，这些命令都安装在python目录下的Script目录里。</p>
<p style="padding-left: 30px;">easy_install和pip作用都差不多，使用的时候help一下就知道其用法了。</p>
3.安装pyramid
<p style="padding-left: 30px;">pip install pyramid</p>
<p style="padding-left: 30px;">这个是在线安装，等待完成后，在python/Lib/site-packages/下面就会发现pyramid以及相关的package了。</p>
4.运行一个Hell world
<p style="padding-left: 30px;">Hello world！多少程序的第一次都给了你啊~~~</p>
<p style="padding-left: 30px;">找个目录，创建一个hello.py，我把pyramid官网上的sample运行一下，把一下代码拷贝到hello.py：</p>

<div style="padding-left: 30px;">
<pre>[python]
from wsgiref.simple_server import make_server
from pyramid.config import Configurator
from pyramid.response import Response

def hello_world(request):
   return Response('Hello %(name)s!' % request.matchdict)

if __name__ == '__main__':
   config = Configurator()
   config.add_route('hello', '/hello/{name}')
   config.add_view(hello_world, route_name='hello')
   app = config.make_wsgi_app()
   server = make_server('0.0.0.0', 8080, app)
   server.serve_forever()
[/python]</pre>
cmd下到hello.py目录下，执行python hello.py，然后在浏览器运行<span style="text-decoration: underline;">http://localhost:8080/hello/world</span>

</div>
关于pyramid的资料可以去官网<a href="http://www.pylonsproject.org" target="_blank">http://www.pylonsproject.org</a>下载。上面只是一个简单的sample，我们做WEB开发当然要比这个复杂一些，接下来就是创建一个pyramid project了。
