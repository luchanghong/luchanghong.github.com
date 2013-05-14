---
layout: post
title: 在Pyramid中使用SESSION
category: python
tags: [python, pyramid, session]
description: 伴随项目的开发，需要使用SESSION来保存用户产生的数据。
---

## SEESION in Pyramid

安装官网上[SESSION文档][1]的描述，一般有两种SESSION Factory和两种关于SESSION的用法。

[1]: http://docs.pylonsproject.org/projects/pyramid/en/1.4-branch/narr/sessions.html

## 默认的SESSION Factory

在`__init__.py`中添加一些代码：

```python
from pyramid.session import UnencryptedCookieSessionFactoryConfig
my_session_factory = UnencryptedCookieSessionFactoryConfig('itsaseekreet')

from pyramid.config import Configurator
config = Configurator(session_factory = my_session_factory)

# 上面一行代码也等价于下面
config.set_session_factory(my_session_factory)
```

## pyramid_beaker

[pyramid_beaker文档][2]参见官网。首先要安装`pyramid_beaker`这个package，可以在setup.py中添加需要的包，然后执行下面

[2]: http://docs.pylonsproject.org/projects/pyramid_beaker/en/latest

```python
# 编辑setup.py
requires = [
    'pyramid',
    'SQLAlchemy',
    'transaction',
    'pyramid_tm',
    'pyramid_debugtoolbar',
    'zope.sqlalchemy',
    'waitress',
    'mako',
    'pyramid_beaker',
    ]
```

```bash
# 执行程序
python setup.py develop
```

有以下三种使用方法：

- 在项目配置文件如develop.ini中添加一个配置

    ```ini
    pyramid.includes =
        pyramid_debugtoolbar
        pyramid_tm
        pyramid_beaker
    ```

- 在__init__.py添加一行：

    ```python
    config.include('pyramid_beaker')
    ```

- 在develop.ini里添加配置

    ```ini
    ###
    # pyramid_beaker
    ###
    session.type = file
    session.data_dir = %(here)s/data/sessions/data
    session.lock_dir = %(here)s/data/sessions/lock
    # key和secert都是自定义的
    session.key = key_lbew
    session.secret = secret_lbew_webl
    session.cookie_on_exception = true
    ```

    然后在__init__.py里的main函数中添加几行配置：

    ```python
    # use session
    from pyramid_beaker import session_factory_from_settings
    session_factory = session_factory_from_settings(settings)
    config.set_session_factory(session_factory)
    ```

## SESSION存储调用

写个测试页面，在views/account.py中code：

```python
__author__ = 'luchanghong'
#!/usr/bin/env python
#-*- coding: utf8 -*-

from pyramid.view import view_config

class Account(object):
    def __init__(self, request):
        self.request = request
        print self.request.session, self.request.session.get('aaa', None), self.request.session.created

    @view_config(route_name = 'signup', renderer = 'string')
    def signup(self):
        self.request.session['aaa'] = 'bbb'
        return 'Welcome to register.'
```

查看输出结果：

```bash
Starting server in PID 11688.
serving on http://0.0.0.0:6543
{'aaa': 'bbb', '_accessed_time': 1368526206.591521, '_creation_time': 1368514712.58489} bbb 1368514712.58
```

可以看出SESSION操作就和字典一样。

## CSRF TOKEN

为了防止恶意表单提交，可以使用SESSION生成一个CSRF TOKEN，在处理表单数据之前做校验：

```python
# 把csrf_token传到页面里做一个隐藏字段
csrf_token = request.session.new_csrf_token()
```
