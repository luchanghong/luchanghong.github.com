--- 
wordpress_url: http://luchanghong.com/rosemary/?p=229
date: 2012-05-18 01:44:45 +08:00
layout: post
wordpress_id: 229
title: !binary |
  RGphbmdv5byA5Y+R5a2m5Lmg77yI5Zub77yJ

---
凌晨快1点了，寂寞的男人不瞌睡。也没什么可做的，想着把剩下一点给写了吧，因为google还是挺给力的，上篇博客发表2个多小时就收录了，反正到周末了也没时间写，那一鼓作气吧~~

一、配置URL

看一下admin的URL，大致就知道怎么配置了。原理很简单，每次用户请求一个URL的时候，Django就会来urls.py找到urlpatterns并匹配里面的URL，找到了就会根据URL调用对应的VIEW，VIEW会返回一个HttpResponse给用户，这就是简单的流程。

URL使用正则表达式来匹配的，Django自带的功能用include()来对应VIEW，对于一个普通的应用，这里用polls，那么就直接写polls.views.index类似的了，index就是polls/views.py里面的一个方法。如：
<pre>[python]
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
    url(r'^polls/(?P&lt;poll_id&gt;\d+)/$', 'polls.views.detail')
)
[/python]</pre>
二、写VIEW方法

写个最简单的VIEW方法，比如：
<pre>[python]
# Create your views here.
from django.http import HttpResponse
from models import Poll

def index(request):
    return HttpResponse('Hello,welcome to polls index!')
[/python]</pre>
先runserver，然后<a href="http://localhost:8000/polls/">http://localhost:8000/polls/</a>

<a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-view.jpg"><img class="alignnone size-full wp-image-230" title="url-view" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-view.jpg" alt="" width="397" height="262" /></a>

然后，来点难度，比如传递参数怎么办？来看GET方式
<pre>[python]
def index(request):
    arg = request.GET.get('act')
    return HttpResponse('Hello,welcome to polls index!&lt;br&gt;The arg is : %s' % arg)
[/python]</pre>
那么加个参数试试：<a href="http://localhost:8000/polls/?act=test">http://localhost:8000/polls/?act=test</a>
<a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-arg-view.jpg"><img class="alignnone size-full wp-image-231" title="url-arg-view" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-arg-view.jpg" alt="" width="428" height="262" /></a>
<pre>另外还有一种参数传递方式，形如/polls/1/这样的，这里“1”是可变的参数，这样的URL就对应下面这种写法：</pre>
<pre>url(r'^polls/(?P&lt;poll_id&gt;\d+)/$', 'polls.views.detail')</pre>
来看VIEW的写法：
<pre>[python]
def detail(request, poll_id):
    return HttpResponse('You are looking at poll %s' % poll_id)
[/python]</pre>
<pre>测试一下：<a href="http://localhost:8000/polls/6/">http://localhost:8000/polls/6/</a></pre>
<pre><a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-arg-view2.jpg"><img class="alignnone size-full wp-image-232" title="url-arg-view2" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-arg-view2.jpg" alt="" width="428" height="262" /></a></pre>
三、渲染模板并赋值变量

使用HTML模板很简单，看一下代码：
<pre>[python]
from django.template import Context,loader
from django.shortcuts import render_to_response

def index(request):
    arg = request.GET.get('act')
    t = loader.get_template('polls/index.html')
    c = Context({
        'arg': arg,
        'list': range(10),
    })
    #return render_to_response('polls/index.html', {'arg': arg, 'list': range(10)})
    return HttpResponse(t.render(c))
[/python]</pre>
用到的index.html路径mysite/template/polls/index.html，和admin的模板同一级别，测试一下：<a href="http://localhost:8000/polls/?act=test">http://localhost:8000/polls/?act=test</a>

<a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-template.jpg"><img class="alignnone size-full wp-image-233" title="url-template" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/url-template.jpg" alt="" width="533" height="422" /></a>

这里，稍微在index.html里面写了一点CSS样式。细心点就会注意到注释的那句话，可以替换那些代码完成同样的功能，可以试一下。

四、在模版输出变量以及循环

上图里面，我输出了传递过来的变量，一个字典：
<pre>[python]{'arg': arg, 'list': range(10)}[/python]</pre>
其实和PHP框架以及之前用的pyramid框架差不多，只是定界符不同。变量直接写在两对花括号输出：{{ arg }} ，循环语句则是{% for x in list %}
<pre>[html]
&lt;!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd"&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;&lt;/title&gt;
    &lt;style type="text/css"&gt;
        body { color: red; text-align: center; background-color: #adff2f; }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    Hello, I am the index template of polls.
    &lt;p&gt;The arg is {{ arg }}&lt;/p&gt;
    &lt;p&gt;{{ list }}&lt;/p&gt;
    {% for x in list %}
        {{ x }}&lt;br&gt;
    {% endfor %}
&lt;/body&gt;
&lt;/html&gt;
[/html]</pre>
大概也就这么多了，不明白的还是去看手册，这里也不可能全面概括到。贴上文件的结构以便参考，如果有什么不对或者错误的，还希望留言指正，以免误导初学者~~~好吧，虽然我也是初学者。

<a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mysite-file.jpg"><img class="alignnone size-full wp-image-237" title="mysite-file" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mysite-file.jpg" alt="" width="221" height="409" /></a>

<span style="color: #ff0000;">PS：上篇文章忘了说把admin的模板替换一下了，admin的模板默认的在本地python目录下lib/site-packages/django/contrib/admin/templates/admin/下面，直接拷贝过来就OK了，Django会先在当前project的templates目录查找admin的模板，如果没有就调用默认的模板，当然还要在settings.py里面配置模板路径（绝对路径）。</span>
<pre>[python]
TEMPLATE_DIRS = (
    'e:/python/django/mysite/templates'
    # Put strings here, like "/home/html/django_templates" or "C:/www/django/templates".
    # Always use forward slashes, even on Windows.
    # Don't forget to use absolute paths, not relative paths.
)
[/python]</pre>
