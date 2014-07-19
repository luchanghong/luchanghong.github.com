--- 
wordpress_id: 251
wordpress_url: http://luchanghong.com/rosemary/?p=251
date: 2012-05-21 15:23:16 +08:00
layout: post
title: Django应用——腾讯开放平台
category: python
tags: [python, django]
description: 学以致用，用 django 来跑一下腾讯开放平台的 API 。
---
上周学习了Django，学以致用，就拿腾讯开放平台举例吧。说到这个腾讯开放平台，其实类似的平台很多，新浪、百度等都有，相对来说腾讯用户量算是最大的了，对于一些开发者来说，通过这些开放平台可以挣点小钱花，这里的应用最火的当然是游戏类的，但都是flash格式的，Python和PHP等都是望尘莫及啊。废话不多说，做个测试吧。

## 一、去腾讯开放平台创建一个应用

用QQ号登录TX开放平台 <a href="http://open.qq.com/">http://open.qq.com/</a>，然后创建一个应用，之后记录APPID和APP KEY（做过接口应用的应该都很熟悉这些流程）。然后去下载SKD <a href="http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/Python_SDK_V3.0.0.zip" target="_blank">http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/doc/Python_SDK_V3.0.0.zip</a> ，这是官方的OPENAPI V3.0.0 SDK。

## 二、在Django项目下创建app

到我们的project下，run command:

```bash
python manage.py startapp qq_open
```

便创建了qq_open目录，把下载的Python SDK解压出来，重命名为qqapi，然后copy到这个目录下面。

首先，把这个应用添加到项目里面去，在settings.py里面，给INSTALLED_APPS添加一个值（‘qq_open’）；

接着配置URLconf，具体做法上篇文章说过，<a title="Django开发学习（五）" href="http://luchanghong.com/rosemary/?p=244" target="_blank">猛击这里</a>，直接看下面步骤：
<ol>
	<li>在qq_open目录下创建urls.py

```python
#-*- coding: utf-8 -*-
__author__ = 'luchanghong'

from django.conf.urls import url,patterns

urlpatterns = patterns('qq_open.views',
    url(r'^$', 'index'),
)
```

</li>
	<li>在mysite/urls.py增加一行：

```python
url(r'^qq_open/', include('qq_open.urls'))
```

</li>
	<li>写VIEW，打开qq_open/views.py：

```python
# Create your views here.
from django.shortcuts import render_to_response

def index(request):
    ret = {'hello': 'I am qq_open test.'}
    return render_to_response('qq_open/index.html', ret)
```

</li>
	<li>创建目录以及文件：templates/qq_open/index.html

```python
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<title>qq open app</title>
</head>
<body>
  { { hello }}<br>
</body>
</html>
```

</li>
</ol>
OK，run command:

```bash
python manage.py runserver
```

然后，可以访问<a href="http://localhost:8000/qq_open/">http://localhost:8000/qq_open/</a>测试一下qq_open有没有运行成功。

## 三、使用OPENAPI SDK

在每一个封装的类里面都有调用的例子，那就简单的调用用户的信息吧——get_user_info。

首先，去腾讯开放平台，打开应用管理-》应用资料，设置QQ空间平台资料，设置应用开发地址：

<a href="/upload/2012/05/qzone-config.jpg"><img class="alignnone size-full wp-image-252" title="qzone-config" src="/upload/2012/05/qzone-config.jpg" alt="" width="518" height="385" /></a>

然后，把VIEW修改一下：

```python
# Create your views here.
from django.shortcuts import render_to_response
from qqapi.openapi_v3 import OpenAPIV3

def index(request):
    ret = {'hello': 'I am qq_open test.'}
    appid = '你的APPID'
    appkey = '你的APP KEY'
    iplist = ('119.147.19.43',)

    # openid = '0000000000000000000000000039811C'
    # openkey = 'EC88754BBE1ADC64A93EB4432514B84B0CC019F3A2759C8C8'
    openid = request.GET.get('openid')
    openkey = request.GET.get('openkey')

    pf = 'qzone'

    api = OpenAPIV3(appid, appkey, iplist)

    jdata = api.call('/v3/user/get_info', {
        'pf': pf,
        'openid': openid,
        'openkey': openkey
    })
    ret.update({'data': jdata})
    return render_to_response('qq_open/index.html', ret)
```

在HTML里面把用户头像显示出来：

```html
<body>
  { { hello }}<br>
  { { data }}<br>
  <img src="{ { data.figureurl }}" alt="">
</body>
```

完成这些，上图中有个“预览应用”，点击预览跳转到QQ空间：

<a href="/upload/2012/05/qzone.jpg"><img class="alignnone size-full wp-image-255" title="qzone" src="/upload/2012/05/qzone.jpg" alt="" width="774" height="425" /></a>

这个流程就是这样的了，注意此时在本地访问<a href="http://localhost:8000/qq_open/">http://localhost:8000/qq_open/</a>肯定得不到结果的，因为少了好几个参数，这些参数是腾讯那边生成的，主要是做验证用的，关于腾讯开放平台的使用，感兴趣的可以去官网网站看看API接口规则等文档。

<a href="/upload/2012/05/qq_open_files.jpg"><img class="alignnone size-full wp-image-258" title="qq_open_files" src="/upload/2012/05/qq_open_files.jpg" alt="" width="213" height="484" /></a>

吐槽一下：这个研究老半天，因为openid和openkey不知道是怎么得到的，最后才明白去调试才能生成，另外，为了测试，我把PHP的那个SDK也跑了一下~！
