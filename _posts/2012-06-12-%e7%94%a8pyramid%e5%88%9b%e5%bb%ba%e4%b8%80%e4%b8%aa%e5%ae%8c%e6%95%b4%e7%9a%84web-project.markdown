--- 
wordpress_url: http://luchanghong.com/rosemary/?p=284
wordpress_id: 284
title: !binary |
  55SocHlyYW1pZOWIm+W7uuS4gOS4quWujOaVtOeahFdFQiBQcm9qZWN0

layout: post
date: 2012-06-12 18:00:51 +08:00
---
之前公司用pyramid做开发，那时候刚开始学习，有很多不懂，都是别人定义好的，我只是拿来用，所以一些原理不是太清楚。最近公司开展新项目，依然用的是pyramid，只是数据库从mongoDB改为mySQL，这个project差不多是我自己来完成的，总结了一下步骤。

一、创建一个pyramid project

我的开发环境：WIN7  32bit + python 2.6.6 + mysql 5.5.20 + mongodb 2.0.3

在项目目录下执行：

[shell]pcreate -s starter myproject[/shell]

这个命令应该很熟悉了吧，pcreate是装了pyramid之后在python/Scripts/目录生成的一个可执行文件，通常把python/Scripts/加入到系统环境变量以方便使用。

然后，以develop的方式来run我们的项目，production.ini则是生产环境（线上）的配置文件：

[shell]python setup.py develop[/shell]

如果项目多人参与开发，那么每个人都可以拷贝一份development.ini根据当前开发环境来配置，然后以此来run项目：

[shell]pserve my_development.ini --reload[/shell]

reload参数说明：当修改项目下的.py文件或者配置文件后pserve自动重启，方便开发调试。

二、配置development.ini

你可以在这里设置一些配置，比如mysql的主机、用户名、密码，debug是否开启，如：
[shell]
; For mysql
mysql.host = localhost
mysql.port = 3306
mysql.user = root
mysql.passwd = root
mysql.db = myproject
mysql.charset = utf8
[/shell]

引用的时候可以这样写：[python]settings['mysql.host'][/python]

数据库的连接状态我们肯定想一直保持，要不然每次都要connect一下很麻烦，所以可以在myproject/__init__.py里面把db_connect放在request里面，方便调用：
<pre>[python]
import pymysql
from pyramid.config import Configurator
from pyramid.events import NewRequest
def main(global_config, **settings):
    config = Configurator(settings=settings)
    # connect mysql
    def add_mysql_db(event):
        db_host = settings['mysql.host']
        db_port = int(settings['mysql.port'])
        db_user = settings['mysql.user']
        db_pass = settings['mysql.passwd']
        db_name = settings['mysql.db']
        db_charset = settings['mysql.charset']
        conn = pymysql.connect(host = db_host, port = db_port, user = db_user,
            passwd = db_pass, db = db_name, charset = db_charset)
        event.request.db = conn.cursor()
    config.add_subscriber(add_mysql_db, NewRequest)
[/python]</pre>
三、route &amp; view

在上面那个__init__.py里面有一个home的route，可以看到写法。route和view是成对出现的，项目里面的route很多，如果都写在这不方便管理，所以我们新建一个文件专门存放route，view不必非要紧挨着route，仔细看配置文件会发现config.scan()，他会帮我们快速配对route和view，通常config.scan(‘myproject’)，应该很容易理解吧（myproject相当于一个package）。

route的写法可以查看pyramid文档，就不在此啰嗦了，后面我把一个完整的配置文件共享出来。

四、renderer一个html模板

pyramid默认使用mako模板引擎，mako默认支持.pt后缀的模板文件，我们常用.html，所以要配置一下，很简单，在上面那个__init__.py的main()函数里加上：

[python]config.add_renderer('.html', 'pyramid.mako_templating.renderer_factory')[/python]

在development.ini文件里制定mako模板路径：
[shell]
; For Mako Template
mako.directories = myproject:templates
mako.strict_undefined = true
[/shell]

五、session factory

关于session，一般设定方式如下：

[python]
import pyramid_beaker
# set session factory
session_factory = pyramid_beaker.session_factory_from_settings (settings)
config.set_session_factory (session_factory)
pyramid_beaker.set_cache_regions_from_settings (settings)
[/python]

也要在development.ini设置一下：

[shell]
; For pyramid_beaker
session.type = file
session.data_dir = %(here)s/data/sessions/data
session.lock_dir = %(here)s/data/sessions/lock
session.key = myproject_session
session.cookie_on_exception = true

;cache.regions = default_term, second, short_term, long_term
;cache.type = memory
;cache.second.expire = 1
;cache.short_term.expire = 60
;cache.default_term.expire = 300
;cache.long_term.expire = 3600
[/shell]

分号是注释作用。

用的话直接在request.session里面取：request.session.get('username')

六、权限系统

这个有点小复杂，可以看手册里面security和resources。资源--权限--角色--用户这个思路，理解起来就是赋予用户某些角色，然后是对资源授权，注意：权限是角色固有的，而非和用户绑定在一起，以后有时间好好分享一下。

以上六步算是比较完整的了。development.ini配置较简单，下面是myproject/__init__.py的配置：
<pre>[python]
import pymysql,pymongo
import pyramid_beaker
from pyramid.config import Configurator
from pyramid.events import NewRequest
from urls import add_web_route

def main(global_config, **settings):
    """ This function returns a Pyramid WSGI application.
    """
    config = Configurator(settings=settings)

    config.add_static_view('static', 'myproject:static', cache_max_age=3600)

    # set session factory
    session_factory = pyramid_beaker.session_factory_from_settings (settings)
    config.set_session_factory (session_factory)
    pyramid_beaker.set_cache_regions_from_settings (settings)

    # render a html template
    config.add_renderer('.pt', 'pyramid.mako_templating.renderer_factory')
    config.add_renderer('.html', 'pyramid.mako_templating.renderer_factory')

    # MongoDB
    def add_mongo_db(event):
        settings = event.request.registry.settings
        db = pymongo.Connection(settings['mongodb.url'])[settings['mongodb.db_name']]
        event.request.mongo_db = db
    config.add_subscriber(add_mongo_db, NewRequest)

    # connect mysql
    def add_mysql_db(event):
        db_host = settings['mysql.host']
        db_port = int(settings['mysql.port'])
        db_user = settings['mysql.user']
        db_pass = settings['mysql.passwd']
        db_name = settings['mysql.db']
        db_charset = settings['mysql.charset']
        conn = pymysql.connect(host = db_host, port = db_port, user = db_user,
            passwd = db_pass, db = db_name, charset = db_charset)
        event.request.db = conn.cursor()
    config.add_subscriber(add_mysql_db, NewRequest)

    # config.add_route('home', '/')
    # add route
    add_web_route(config)
    config.scan('myproject')
    return config.make_wsgi_app()
[/python]</pre>
下面是myproject/urls.py:
<pre>[python]
# -*- coding: utf-8 -*-
__author__ = 'luchanghong'

def add_web_route(config):
    # web common
    config.add_route (name = 'web.index', pattern = '/')
[/python]</pre>
下面是development.ini:

[shell]

[app:main]
use = egg:myproject

pyramid.reload_templates = true
pyramid.debug_authorization = false
pyramid.debug_notfound = false
pyramid.debug_routematch = false
pyramid.default_locale_name = en
; pyramid.includes = pyramid_debugtoolbar

; For pyramid_beaker
session.type = file
session.data_dir = %(here)s/data/sessions/data
session.lock_dir = %(here)s/data/sessions/lock
session.key = myproject_session
session.cookie_on_exception = true

;cache.regions = default_term, second, short_term, long_term
;cache.type = memory
;cache.second.expire = 1
;cache.short_term.expire = 60
;cache.default_term.expire = 300
;cache.long_term.expire = 3600

; For Mako Template
mako.directories = myproject:templates
mako.strict_undefined = true

; For Mongo
mongodb.url = mongodb://127.0.0.1
mongodb.db_name = myproject

; For mysql
mysql.host = localhost
mysql.port = 3306
mysql.user = root
mysql.passwd = root
mysql.db = myproject
mysql.charset = utf8

[server:main]
use = egg:waitress#main
host = 0.0.0.0
port = 6543

# Begin logging configuration

[loggers]
keys = root, myproject

[handlers]
keys = console

[formatters]
keys = generic

[logger_root]
level = INFO
handlers = console

[logger_myproject]
level = DEBUG
handlers =
qualname = myproject

[handler_console]
class = StreamHandler
args = (sys.stderr,)
level = NOTSET
formatter = generic

[formatter_generic]
format = %(asctime)s %(levelname)-5.5s [%(name)s][%(threadName)s] %(message)s

# End logging configuration

[/shell]

注意我的项目名称是：myproject
