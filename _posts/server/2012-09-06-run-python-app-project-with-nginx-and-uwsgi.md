---
layout: post
title: Nginx与uWSGI部署python应用
category: server
tags: [nginx, uwsgi]
description: 在网上看到 python 应用的一些部署方式，如今最高效的当属 uWSGI 了，今天就在本地部署感受一下。
---

## 安装 Nginx

    sudo brew install nginx

路径：`/usr/local/sbin/nginx`，为了方便启动，把 `/usr/local/sbin/` 加入到 PATH 中去，或者把 nginx 做一个 link:

    sudo ln -s /usr/local/sbin/nginx /usr/bin/nginx

在我 mac 电脑上装完 nginx 之后，它自己就启动了，打开 `localhost:9000` ，就会看到 `Welcome to nginx!`。

## 安装 uwsgi

最好先创建一个虚拟环境，可以用 virtualenv ，我用的是 [pythonbrew][]

写一个简单的 app ，然后用 uwsgi 来启动。我的路径是 `/Users/lch/dev/www/python`，创建一个文件 `hello.py` ：

<pre class="prettyprint">
#!/usr/bin/env python
#-*- coding: utf8 -*-

def application(env, response):
    response('200 OK', [('Content-Type', 'text/html')])
    return "Hello World"
</pre>

用 uwsgi 来启动：`uwsgi --socket :9000 --wsgi-file hello.py -t 30`

## 配置nginx

现在我配置一个新的 server ，让 nginx 做 8000 到 9000 端口的转发。在 nginx 配置文件加上一段：

<pre class="prettyprint">
upstream hello-web-app{
    server localhost:9000;
}

server{
    listen 8000;
    server_name localhost;

    access_log /Users/lch/dev/www/log/nginx/access.log;
    error_log /Users/lch/dev/www/log/nginx/error.log;

    location / { 
        include uwsgi_params;
        uwsgi_pass hello-web-app;
        keepalive_timeout 0;
    }   
}
</pre>

然后重启 nginx ，访问 `localhost:8000` 就会显示 `Hello World`了，我们的配置成功了，也可以通过 log 来检测。

在网上看到一些相关文章的介绍，但是有一些出入，都没有成功，需要注意一下几点：

- 网上普遍 `uwsgi_pass localhost:9000`是不行的，得写成 upstream 的形式
- uwsgi 启动 app 的时候，是 socket ，而不是 http ，否则 nginx 转发不过去
- nginx 的一些知识要有所准备

我用的软件版本信息：
<pre class="prettyprint">
(env_uwsgi)lchmatoMacBook-Pro:python lch$ nginx -v
nginx version: nginx/1.2.3
(env_uwsgi)lchmatoMacBook-Pro:python lch$ uwsgi --version
1.2.5
</pre>

[pythonbrew]: http://localhost:4000/python/2012/07/23/switch-your-python-env-with-using-pythonbrew.html "pythonbrew"
