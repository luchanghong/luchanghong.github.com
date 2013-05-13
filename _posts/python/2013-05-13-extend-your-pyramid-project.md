---
layout: post
title: 扩展你的Pyramid项目
category: python
tags: [python, pyramid]
description: 当我们创建好一个基本的Pyramid项目之后，分别只有一个views.py、models.py，而且route关系定义在__init__.py里面，当项目逻辑关系很复杂的时候，这几个文件是很难维护的，而且不利于团队协同开发，下面就看看如何扩展Pyramid项目。
---

以下内容均以[上一篇文章][1]为基础，项目名称为 **lbew** 。

[1]: http://luchanghong.com/python/2013/05/10/use-mysql-and-sqlalchemy-url-dispatch-in-pyramid-project.html "使用MySQL和SQLAlchemy+URL Dispatch构建Pyramid应用"

## 扩展route

打开`lbew/__init__.py`，有一行配置：

```python
config.add_route('home', '/')
```

一般来说，网站的route不会只有一个，比如登录、注册、会员中心……建立一个专门存放route的文件是很有用的。那么就在__init__.py所在目录创建routes.py，定义一个函数如下：

```python
__author__ = 'luchanghong'
#!/usr/bin/env python
#-*- coding:utf8 -*-

def lbew_routes(config):
    # index & test
    config.add_route(name = 'index',    pattern = '/index.html')
    config.add_route(name = 'test',     pattern = '/test')
```

在__init__.py中把这些配置导入进来：

```python
from pyramid.config import Configurator
from sqlalchemy import engine_from_config
from .routes import lbew_routes

from .models import (
    DBSession,
    Base,
    )

def main(global_config, **settings):
    """ This function returns a Pyramid WSGI application.
    """
    engine = engine_from_config(settings, 'sqlalchemy.')
    DBSession.configure(bind=engine)
    Base.metadata.bind = engine
    config = Configurator(settings=settings)
    config.add_static_view('static', 'static', cache_max_age=3600)
    config.add_route('home', '/')
    lbew_routes(config)
    config.scan()
    return config.make_wsgi_app()
```

如果你的项目比较庞大，可以把每一个模块单独配置一个route文件，就像下面的views和models这样扩展。

## 扩展view

在views.py所在目录创建文件夹views，然后把views.py拷贝到views并命名为__init__.py并做一处修改：

```python
from ..models import (
    DBSession,
    MyModel,
    )
```

因为此时__init__.py相对于models.py多一层路径。如果你觉得这样比较容易出错的话，可以这样写：

```python
from lbew.models import (
    DBSession,
    MyModel,
    )
```

因为项目本身就是一个package。那么怎么才能体现出扩展呢？在views目录下创建文件account.py，专门做用户账号登录、注册、修改密码等系列操作。

- 在routes.py里面添加一个route：

    ```python
    config.add_route(name = 'signup',   pattern = '/account/signup')
    ```

- 在account.py中code

    ```python
    __author__ = 'luchanghong'
    #!/usr/bin/env python
    #-*- coding: utf8 -*-

    from pyramid.view import view_config

    class Account(object):
        def __init__(self, request):
            self.requestt = request

        @view_config(route_name = 'signup', renderer = 'string')
        def signup(self):
            return 'Welcome to register.'
    ```

- 重启pserve，浏览器访问`http://0.0.0.0:6543/account/signup`即可看到相应输出。

## 扩展model

同样建立models文件夹并把models.py移动过来重命名为__init__.py。新建文件account.py并code：

```python
__author__ = 'luchangong'
#!/usr/bin/env python
#-*- coding: utf8 -*-

from lbew.models import Base
from sqlalchemy import (
    Column,
    Integer,
    String,
    )

class Account(Base):
    __tablename__ = 'account'
    id = Column(Integer, primary_key=True)
    name = Column(String(20), unique=True)
    value = Column(Integer)

    def __init__(self, name, value):
        self.name = name
        self.value = value
```

在部署的时候要初始化数据库：

```bash
(env_lbew)lch@Mac-Pro-LCH:lbew $ initialize_lbew_db development.ini
```

这时候会出现两个问题，一个是models表主键冲突；一个是account这个数据表没有建立，下面解决这两个问题。

- 主键冲突是因为`lbew/scripts/initializedb.py`最后几行：

    ```python
    with transaction.manager:
        model = MyModel(name='one', value=1)
        DBSession.add(model)
    ```

    解决办法就是注释掉。

- 如何根据新的数据model创建对应的数据表。研究一下代码，在`modles/__init__.py`里添加一行：

    ```python
    from account import Account
    ```

    然后在执行`initialize_lbew_db development.ini`即可。

OK，把扩展做好之后接下来看SQLAlchemy这个ORM对MySQL的数据操作。
