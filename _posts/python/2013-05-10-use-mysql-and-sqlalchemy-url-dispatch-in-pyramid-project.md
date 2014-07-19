---
layout: post
title: 使用MySQL和SQLAlchemy+URL Dispatch构建Pyramid应用
category: python
tags: [python, pyramid, sqlalchemy]
description: 最近想用pyramid框架开发一个项目，好久也没有去关注pyramid了，去官网看一下已经更新到1.4版本。刚好前几天有网友问我相关问题，就来亲自做一下。
---

## 创建项目

在开发之前最好创建一个虚拟的python环境，这里我使用的是[pythonbrew][pythonbrew]来实现：

```bash
$ pythonbrew venv create env_lbew
```

然后[安装pyramid][install pyramid]，完成之后启用虚拟环境并且创建一个`alchemy scaffold`项目：

```bash
lch@localhost:mine $ pythonbrew venv use env_lbew
# Using `env_lbew` environment (found in /Users/lch/.pythonbrew/venvs/Python-2.7.3)
# To leave an environment, simply run `deactivate`
(env_lbew)lch@localhost:mine $ pcreate -s alchemy lbew
```

[pythonbrew]: http://luchanghong.com/python/2012/07/22/switch-your-python-env-with-using-pythonbrew.html "switch your python env with using pythonbrew"
[install pyramid]: http://luchanghong.com/python/2012/04/07/install-pyramid-and-run-a-sample-hello-world.html "安装pyramid并运行一个hello world"

## 启动项目

首先通过下面命令来安装项目依赖的类库，这一步是必不可少的，因为之后会生成一个部署数据库的脚本，在当前python环境的bin目录下，和`pcreate/pserve`同一目录，命名方式是：initialize_projectName_db

```bash
(env_lbew)lch@localhost:lbew $ python setup.py develop
(env_lbew)lch@localhost:lbew $ ls ~/.pythonbrew/venvs/Python-2.7.3/env_lbew/bin
activate        activate_this.py    easy_install-2.7    pcreate         prequest        pshell          pygmentize
activate.csh        bfg2pyramid     initialize_lbew_db  pip         proutes         ptweens         python
activate.fish       easy_install        mako-render     pip-2.7         pserve          pviews
```

然后要初始化一下数据库：

```bash
(env_lbew)lch@localhost:lbew $ initialize_lbew_db development.ini
2013-05-11 00:08:40,712 INFO  [sqlalchemy.engine.base.Engine][MainThread] PRAGMA table_info("models")
2013-05-11 00:08:40,712 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
2013-05-11 00:08:40,712 INFO  [sqlalchemy.engine.base.Engine][MainThread]
CREATE TABLE models (
    id INTEGER NOT NULL,
    name TEXT,
    value INTEGER,
    PRIMARY KEY (id),
    UNIQUE (name)
)


2013-05-11 00:08:40,713 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
2013-05-11 00:08:40,714 INFO  [sqlalchemy.engine.base.Engine][MainThread] COMMIT
2013-05-11 00:08:40,716 INFO  [sqlalchemy.engine.base.Engine][MainThread] BEGIN (implicit)
2013-05-11 00:08:40,717 INFO  [sqlalchemy.engine.base.Engine][MainThread] INSERT INTO models (name, value) VALUES (?, ?)
2013-05-11 00:08:40,717 INFO  [sqlalchemy.engine.base.Engine][MainThread] ('one', 1)
2013-05-11 00:08:40,718 INFO  [sqlalchemy.engine.base.Engine][MainThread] COMMIT
```

接着就可以启动项目了：

```bash
(env_lbew)lch@localhost:lbew $ pserve development.ini --reload
Starting subprocess with file monitor
Starting server in PID 4037.
serving on http://0.0.0.0:6543
```

## 使用MySQL数据库

打开`development.ini`可以看到项目默认使用的是`SQLite`数据库，现在改成`MySQL`：

```ini
# sqlalchemy.url = sqlite:///%(here)s/lbew.sqlite
sqlalchemy.url = mysql://root:root@localhost/lbew
# mysql://DB_User:DB_Passwd@DB_Host/DB_Database
```

在MySQL里创建数据库并命名`lbew`，再次初始化数据库：

```bash
(env_lbew)lch@localhost:lbew $ initialize_lbew_db development.ini

... # 省略一些错误输出信息

ImportError: No module named MySQLdb
```

显然，我们需要[安装MySQLdb][install MySQLdb]，安装完毕后再次执行，貌似又有问题：

```bash
  File "build/bdist.macosx-10.7-x86_64/egg/MySQLdb/connections.py", line 36, in defaulterrorhandler
  sqlalchemy.exc.OperationalError: (OperationalError) (1170, "BLOB/TEXT column 'name' used in key specification without a key length") '\nCREATE TABLE models (\n\tid INTEGER NOT NULL AUTO_INCREMENT, \n\tname TEXT, \n\tvalue INTEGER, \n\tPRIMARY KEY (id), \n\tUNIQUE (name)\n)\n\n' ()
```

[install MySQLdb]: http://luchanghong.com/database/2012/07/22/install-mysqldb-package-with-mac.html "Mac下安装MySQLdb"

原因是初始化的项目里的例子（models.py）把`name`字段设置成`TEXT`类型，而且作为`INDEX`，在`SQLite`里是OK的，但是在`MySQL`里`TEXT`类型的字段不能做UNIQUE索引的，在这里做一些修改即可：

```python
from sqlalchemy import (
    Column,
    Integer,
    Text,
    String,
    )

from sqlalchemy.ext.declarative import declarative_base

from sqlalchemy.orm import (
    scoped_session,
    sessionmaker,
    )

from zope.sqlalchemy import ZopeTransactionExtension

DBSession = scoped_session(sessionmaker(extension=ZopeTransactionExtension()))
Base = declarative_base()


class MyModel(Base):
    __tablename__ = 'models'
    id = Column(Integer, primary_key=True)
    name = Column(String(20), unique=True)
    value = Column(Integer)

    def __init__(self, name, value):
        self.name = name
        self.value = value
```

然后再次初始化数据库：

```bash
(env_lbew)lch@localhost:lbew $ initialize_lbew_db development.ini
2013-05-11 00:44:20,059 INFO  [sqlalchemy.engine.base.Engine][MainThread] SELECT DATABASE()
2013-05-11 00:44:20,059 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
2013-05-11 00:44:20,062 INFO  [sqlalchemy.engine.base.Engine][MainThread] SHOW VARIABLES LIKE 'character_set%%'
2013-05-11 00:44:20,063 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
2013-05-11 00:44:20,063 INFO  [sqlalchemy.engine.base.Engine][MainThread] SHOW VARIABLES LIKE 'sql_mode'
2013-05-11 00:44:20,063 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
2013-05-11 00:44:20,064 INFO  [sqlalchemy.engine.base.Engine][MainThread] DESCRIBE `models`
2013-05-11 00:44:20,064 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
2013-05-11 00:44:20,065 INFO  [sqlalchemy.engine.base.Engine][MainThread] ROLLBACK
2013-05-11 00:44:20,066 INFO  [sqlalchemy.engine.base.Engine][MainThread]
CREATE TABLE models (
    id INTEGER NOT NULL AUTO_INCREMENT,
    name VARCHAR(20),
    value INTEGER,
    PRIMARY KEY (id),
    UNIQUE (name)
)


2013-05-11 00:44:20,066 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
2013-05-11 00:44:20,236 INFO  [sqlalchemy.engine.base.Engine][MainThread] COMMIT
2013-05-11 00:44:20,238 INFO  [sqlalchemy.engine.base.Engine][MainThread] BEGIN (implicit)
2013-05-11 00:44:20,239 INFO  [sqlalchemy.engine.base.Engine][MainThread] INSERT INTO models (name, value) VALUES (%s, %s)
2013-05-11 00:44:20,239 INFO  [sqlalchemy.engine.base.Engine][MainThread] ('one', 1)
2013-05-11 00:44:20,240 INFO  [sqlalchemy.engine.base.Engine][MainThread] COMMIT
```

现在去数据库看看是否已经创建好了数据表：

```mysql
(env_lbew)lch@localhost:lbew $ mysql -u root -p lbew
Enter password:
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 110
Server version: 5.5.25a MySQL Community Server (GPL)

Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show tables;
+----------------+
| Tables_in_lbew |
+----------------+
| models         |
+----------------+
1 row in set (0.00 sec)

mysql> select * from models;
+----+------+-------+
| id | name | value |
+----+------+-------+
|  1 | one  |     1 |
+----+------+-------+
1 row in set (0.00 sec)
```

## URL Dispatch

看下面来自官网对`URL Dispatch`的解释，主要作用是解决route和view的映射关系。

> URL dispatch provides a simple way to map URLs to view code using a simple pattern matching language. An ordered set of patterns is checked one-by-one. If one of the patterns matches the path information associated with a request, a particular view callable is invoked. A view callable is a specific bit of code, defined in your application, that receives the request and returns a response object.

下面来增加一对route-view。打开`__init__.py`和`views.py`分别加上：

```python
# __init__.py
config.add_route('home', '/')
# 添加下面一个route配置
config.add_route('test', '/test')


# views.py
# 在文件结尾添加对应的view
@view_config(route_name = 'test', renderer = 'string')
def test(request):
    return 'Hi, I am a test page.'
```

重启应用，浏览`http://0.0.0.0:6543/test`即可看到页面输出相应内容了。
