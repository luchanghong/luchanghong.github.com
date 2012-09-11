---
layout: post
title: 使用supervisor作为uWSGI的守护进程（续）
category: server
tags: [supervisor, uwsgi]
description: 使用 supervisor 并不是很复杂，用它来启动 uWSGI ，但是在测试的过程中还是出了一些小问题，却在 log 上看不出什么原因，慢慢琢磨之后才找到根源。
---

启动 supervisor 之后，用 supervisorctl 来停止一个 program 也很正常，但再次启动的时候就出错了，一直以为是 supervisor 的配置有问题，看一些 supervisor 的 log ：

    2012-09-11 11:29:58,226 INFO supervisord started with pid 951
    2012-09-11 11:29:59,230 INFO spawned: 'push_web_app' with pid 1203
    2012-09-11 11:29:59,288 INFO exited: push_web_app (exit status 1; not expected)
    2012-09-11 11:30:00,305 INFO spawned: 'push_web_app' with pid 1204
    2012-09-11 11:30:00,316 INFO exited: push_web_app (exit status 1; not expected)
    2012-09-11 11:30:02,320 INFO spawned: 'push_web_app' with pid 1205
    2012-09-11 11:30:02,331 INFO exited: push_web_app (exit status 1; not expected)
    2012-09-11 11:30:05,337 INFO spawned: 'push_web_app' with pid 1207
    2012-09-11 11:30:05,348 INFO exited: push_web_app (exit status 1; not expected)
    2012-09-11 11:30:06,350 INFO gave up: push_web_app entered FATAL state, too many start retries too quickly

不过配置文件怎么修改也不行，觉得还是 uwsgi 有问题，查看一下 uWSGI 的 log :

    *** Starting uWSGI 1.2.5 (64bit) on [Tue Sep 11 11:30:49 2012] ***
    compiled with version: 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2336.11.00) on 06 September 2012 14:23:41
    detected number of CPU cores: 4
    current working directory: /Users/lch
    detected binary path: /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_uwsgi/bin/uwsgi
    your memory page size is 4096 bytes
    detected max file descriptor number: 2560 
    lock engine: OSX spinlocks
    probably another instance of uWSGI is running on the same address.
    bind(): Address already in use [socket.c line 751] 
    supervisor: error trying to setuid to 501 (Can't drop privilege as nonroot user)

倒数第二句看到了错误原因，于是把 supervisor 和 uwsgi 全部 kill 掉，再重新启动 supervisor，然后 stop ，通过 `ps aux | grep uwsgi` 发现居然还有一个 `uwsgi process` 没有结束掉，于是手动 `sudo kill pid` 结束这个进程，然后再通过 supervisorctl 启动，终于OK了。

到这里算是找到了问题的原因，也明确了解决方法：停止 supervisor 的时候结束所有的 uwsgi 进程，根源就是 uwsgi 启动时候的配置有问题。我给 uwsgi 配置的启动参数 workers 是 2 ，所有会启动 2 个 uwsgi process ，而通过 supervisorctl 停止的时候只会结束掉一个，那么肯定要有一个主进程来控制着他们了，不经意间看到 uwsgi 启动 log 里面一个 WARNING ：

    *** Starting uWSGI 1.2.5 (64bit) on [Tue Sep 11 10:53:17 2012] ***
    compiled with version: 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2336.11.00) on 06 September 2012 14:23:41
    detected number of CPU cores: 4
    current working directory: /Users/lch
    detected binary path: /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_uwsgi/bin/uwsgi
    *** WARNING: you are running uWSGI without its master process manager ***

于是，我加了一个 `--master 1` 的参数，另外还要加一个参数 `--no-orphans` ，它的作用就是在结束掉 `master process` 之后也 kill 掉所有的 `uwsgi process`。修改完毕之后再次 kill 掉所有相关进程，然后再来一遍：
    supervisord ——> supervisorctl stop ——> supervisorctl start
一切正常，即时监控进程也都正常。

至此，关于 Nginx + uWSGI + Pyramid + Supervisor 部署 WEB 的学习要告一段落了。
