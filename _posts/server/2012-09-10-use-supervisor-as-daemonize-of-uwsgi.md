---
layout: post
title: 使用supervisor作为uWSGI的守护进程
category: server
tags: [uwsgi, supervisor]
description: 每次启动 uWSGI 都要输入一长串的命令，觉得很浪费时间，如何方便有效的管理 uWSGI 是值得探讨的一个问题。在折腾一下午之后终于有点成效。
---

## 安装 supervisor

使用 pip 或者 easy_install 命令来安装，前提当然是要有 python 环境，我把 supervisor 和 uWSGI 装在同一个虚拟环境里。

```bash
pip install supervisor
```

## 配置一个 program

安装完成之后，可以使用 python path 里面的命令 echo_supervisor_conf ，用它得到一个默认配置：

```bash
echo_supervisor_conf > supervisor.conf
```

把配置文件放在任意文件夹都行，只要有访问权限即可。为了测试，基本配置几乎不用修改，只需要修改一下 supervisor log 的路径，然后配置一个 program ，如下：

```ini
[program:push_web_app]
user=lch
command=/Users/lch/.pythonbrew/venvs/Python-2.6.7/env_uwsgi/bin/uwsgi --paste config:/Users/lch/work/adview/push/web/airpush/luchanghong.ini --workers 2 -H /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_push/ --socket :9000 --disable-logging
process_name=%(program_name)s ; process_name expr (default %(program_name)s)
numprocs=1                    ; number of processes copies to start (def 1)
stopsignal=QUIT               ; signal used to kill process (default TERM)
redirect_stderr=true          ; redirect proc stderr to stdout (default false)
stdout_logfile=/Users/lch/dev/www/log/uwsgi/app_push.log        ; stdout log path, NONE for none; default AUTO
```

一些默认配置就不写了。为了方便管理，可以把上面的配置单独提取出来，在主配置文件 supervisor.conf 里面包含一下即可。

PS: 使用了 supervisor ，就不要 --daemonize 这个参数了，换成 stdout_logfile 。

## 调试 supervisor

1.通过 supervisord 命令来启动 supervisor 

```bash
supervisord -c /etc/supervisor/supervisord.conf
```

启动成功时监控一下 uWSGI 和 supervisor 的 log:

supervisor 的 log ：

```ini
2012-09-10 16:07:14,877 WARN Included extra file "/etc/supervisor/conf.d/push_web_app.conf" during parsing
2012-09-10 16:07:14,877 INFO Increased RLIMIT_NOFILE limit to 1024
2012-09-10 16:07:14,907 INFO RPC interface 'supervisor' initialized
2012-09-10 16:07:14,907 CRIT Server 'unix_http_server' running without any HTTP authentication checking
2012-09-10 16:07:14,908 INFO daemonizing the supervisord process
2012-09-10 16:07:14,909 INFO supervisord started with pid 2671
2012-09-10 16:07:15,912 INFO spawned: 'push_web_app' with pid 2673
2012-09-10 16:07:16,939 INFO success: push_web_app entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
```

uWSGI 的 log ：

```ini
*** Starting uWSGI 1.2.5 (64bit) on [Mon Sep 10 16:07:15 2012] ***
compiled with version: 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2336.11.00) on 06 September 2012 14:23:41
detected number of CPU cores: 4
current working directory: /Users/lch
detected binary path: /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_uwsgi/bin/uwsgi
*** WARNING: you are running uWSGI without its master process manager ***
your memory page size is 4096 bytes
detected max file descriptor number: 1024
lock engine: OSX spinlocks
uwsgi socket 0 bound to TCP address :9000 fd 3
Python version: 2.6.7 (r267:88850, Jul 23 2012, 10:42:33)  [GCC 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2336.9.00)]
Set PythonHome to /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_push/
*** Python threads support is disabled. You can enable it with --enable-threads ***
Python main interpreter initialized at 0x7faad1c085f0
your server socket listen backlog is limited to 100 connections
*** Operational MODE: preforking ***
Loading paste environment: config:/Users/lch/work/adview/push/web/airpush/luchanghong.ini
WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0x7faad1c085f0 pid: 2673 (default app)
*** uWSGI is running in multiple interpreter mode ***
spawned uWSGI worker 1 (pid: 2673, cores: 1)
spawned uWSGI worker 2 (pid: 2676, cores: 1)
```

2.通过 supervisorctl 命令来停止 supervisor

```bash
(env_uwsgi)lch@LCH:~ $ supervisorctl -c /etc/supervisor/supervisord.conf 
push_web_app                     RUNNING    pid 3023, uptime 0:05:31
supervisor> ?
default commands (type help <topic>):
=====================================
add    clear  fg        open  quit    remove  restart   start   stop  update 
avail  exit   maintail  pid   reload  reread  shutdown  status  tail  version
supervisor> stop push_web_app 
push_web_app: stopped
supervisor> start push_web_app 
```

也可以直接执行：

```bash
lch@LCH:~ $ supervisorctl -c /etc/supervisor/supervisord.conf stop push_web_app
push_web_app: stopped
lch@LCH:~ $ supervisorctl -c /etc/supervisor/supervisord.conf start push_web_app
push_web_app: started
```

在使用 supervisor 作为 daemonize 之后，它会自动监控 uWSGI ，如果手动停止 uWSGI ，supervisor 会帮你重启。

## 问题 problem

用 supervisorctl 停止当前的 program 之后，再 start 却出现了错误：

```bash
lch@LCH:~ $ supervisorctl -c /etc/supervisor/supervisord.conf restart all
push_web_app: ERROR (abnormal termination)
```

具体解决办法下篇文章讨论。
