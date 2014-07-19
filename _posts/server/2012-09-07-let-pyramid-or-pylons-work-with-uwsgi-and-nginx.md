---
layout: post
title: 用Nginx+uWSGI来部署Pyramid/Pylons APP
category: server
tags: [pyramid, uwsgi, nginx, python]
description: 昨天已经部署了一个简单的 Nginx + uWSGI python 应用。今天来学习一下怎样去部署一个真正的项目，我用的 WEB FrameWork 是 Pyramid 。
---

首先搞清楚 `Python Paste` 、`WSGI` 、`uWSGI` 的关系，下面引用某大神的话：

>Paste 是一个封装的 Python HTTP Server ，但是这样的 HTTP Server 不能够胜任生产环境； 而 WSGI 把逻辑与 HTTP REQUEST 分开来，以便我们的 WEB APP 能够适用不同的 HTTP SERVER 比如：Nginx ，这样就可以让 Paste 更专心的处理后台逻辑；uWSGI 是在 WSGI 的基础上用 C 语言开发的，效率更高。

具体可以参考 wiki [Python Paste][] 、[uWSGI 官网][] ，下面来部署一个 Pyramid APP。

上篇文章已经做了一个简单的 Nginx + uWSGI 的部署，那么就在此基础上把 Pyramid App 用 uWSGI 启动并绑定到 9000 端口，就可一用 `http://localhost:8000` 来访问了。

由于 Pyramid 是用 Paste 启动的，在 uWSGI 的官网上看到相关介绍，用下面的方式启动：

```bash
uwsgi --paste config:/Users/lch/work/adview/push/web/airpush/luchanghonini --socket :9000 --workers 4
```

下面是日志：

```bash
(env_push)lchmatoMacBook-Pro:airpush lch$ uwsgi --paste config:/Users/lch/work/adview/push/web/airpush/ 
luchanghong.ini --socket :9000 --workers 4
*** Starting uWSGI 1.2.6 (64bit) on [Fri Sep  7 12:08:06 2012] ***
compiled with version: 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2336.11.00) on 07 September 
2012 09:35:40
detected number of CPU cores: 4
current working directory: /Users/lch/work/adview/push/web/airpush
detected binary path: /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_push/bin/uwsgi
*** WARNING: you are running uWSGI without its master process manager ***
your memory page size is 4096 bytes
detected max file descriptor number: 2560
lock engine: OSX spinlocks
uwsgi socket 0 bound to TCP address :9000 fd 3
Python version: 2.6.7 (r267:88850, Jul 23 2012, 10:42:33)  [GCC 4.2.1 (Based on Apple Inc. build 5658)
 (LLVM build 2336.9.00)]
*** Python threads support is disabled. You can enable it with --enable-threads ***
Python main interpreter initialized at 0x7fb158c08450
your server socket listen backlog is limited to 100 connections
*** Operational MODE: preforking ***
Loading paste environment: config:/Users/lch/work/adview/push/web/airpush/luchanghong.ini
WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0x7fb158c08450 pid: 1739 (default app)
*** uWSGI is running in multiple interpreter mode ***
spawned uWSGI worker 1 (pid: 1739, cores: 1)
spawned uWSGI worker 2 (pid: 1742, cores: 1)
spawned uWSGI worker 3 (pid: 1743, cores: 1)
spawned uWSGI worker 4 (pid: 1744, cores: 1)
```

由于我是在当前 virtualenv 环境下执行的，就用指定 env 的路径，否则要加一个 `-H /usr/python_env/path` 参数，
启动成功之后，可以 `ps -ef | grep uwsgi` 看一下是不是有4个 uWSGI 进程，不加 `--workers` 参数默认启动一个。
如果你不想看到 uwsgi 的及时 log ，可以加这样一个参数 `--daemonize /Users/lch/dev/www/log/uwsgi/app_push.log` 把 log 存放在一个文件中，这样启动完 uWSGI 以后就不会霸占终端了。

注意：`production.ini/develop.ini` 必须是绝对全路径，否则提示：

```bash
ValueError: Cannot resolve relative uri 'config:luchanghong.ini'; no relative_to keyword argument given
```

目前只是简单的把应用启动了，关键的配置参数、调优等还是要在实际项目中学习的。

[Python Paste]: http://en.wikipedia.org/wiki/Python_Paste "Python Paste"
[uWSGI 官网]: http://projects.unbit.it/uwsgi/ "uWSGI 官网"
