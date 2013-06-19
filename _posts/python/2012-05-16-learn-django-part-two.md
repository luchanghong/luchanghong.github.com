--- 
wordpress_id: 210
wordpress_url: http://luchanghong.com/rosemary/?p=210
date: 2012-05-16 18:26:49 +08:00
layout: post
title: Django开发学习（二）
category: python
tags: [python, django]
description: 学习一下 django 中 project 和 app 的关系，以及如何在一个 project 中创建 app 。
---
昨天的学习算是创建了一个project，继续学习如何创建app以及数据关系模型对象。说道project和app，Django是这样解释的：

What’s the difference between a project and an app? An app is aWeb application that does something – e.g., aWeblog
system, a database of public records or a simple poll app. A project is a collection of conﬁguration and apps for a
particular Web site. A project can contain multiple apps. An app can be in multiple projects.

大概意思就是：一个project可以包含多个app，一个app可以被多个project拥有，说明Django里面的app是独立的，可以被复用。

一、创建一个app

以下就以官网上的poll投票应用为例。在mysite目录下：

```bash
python manage.py startapp polls
```

这样就创建了polls目录，包含：__init__.py、models.py、tests.py、views.py。接着编辑models.py，创建两个数据模型对象：poll和choice。

```python
from django.db import models
from datetime import timedelta
from django.utils import timezone

# Create your models here.
class Poll(models.Model):
    question = models.CharField(max_length = 200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    poll = models.ForeignKey(Poll)
    choice = models.CharField(max_length = 200)
    votes = models.IntegerField()
```

很明显，question、pub_date等属性都是要设置的字段，CharField等表示字段的类型，而ForeignKey指的是关联字段，两个数据模型分别代表两张表。

二、激活app并创建数据表

下面就是要在当前的project中激活polls这个app，还记得Django自带的功能是怎么导入的吧？在配置文件中加入polls：

```python
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls',
    # Uncomment the next line to enable the admin:
    'django.contrib.admin',
    # Uncomment the next line to enable admin documentation:
    # 'django.contrib.admindocs',
)
```

接下来就是把建立的数据模型对象转换成对应的数据表，run command：

```bash
python manage.py sql polls
```

<a href="/upload/2012/05/django-app.jpg"><img class="alignnone size-full wp-image-211" title="django-app" src="/upload/2012/05/django-app.jpg" alt="" width="681" height="339" /></a>

上图的SQL语句应该很熟悉吧，不过还没真正的执行，只是显示出来，所以数据库还是没有变化的。在执行之前检查一下错误，然后在执行：

```bash
python manage.py validate
python manage.py syncdb
```

关于上面那个manage.py sql命令，感兴趣的可以看下面几个相关的：

 >If you’re interested, also run the following commands:
• python manage.py validate – Checks for any errors in the construction of your models.
• python manage.py sqlcustom polls – Outputs any custom SQL statements (such as table modiﬁca-
tions or constraints) that are deﬁned for the application.
• python manage.py sqlclear polls – Outputs the necessary DROP TABLE statements for this app,
according to which tables already exist in your database (if any).
• python manage.py sqlindexes polls – Outputs the CREATE INDEX statements for this app.
• python manage.py sqlall polls – A combination of all the SQL from the sql, sqlcustom, and
sqlindexes commands.

执行成功之后，去数据库看一下创建了polls_poll和polls_choice这两个表。

<a href="/upload/2012/05/Django-table.jpg"><img class="alignnone size-full wp-image-212" title="Django-table" src="/upload/2012/05/Django-table.jpg" alt="" width="573" height="379" /></a>

三、使用API

接着使用Django提供的API，并且完成一些数据模型对象的操作。run command:

```bash
python manage.py shell
```

进入一个shell，相当于python的shell，不同的是Django自动把app加入到python path中，也就是说可以直接把app当作一个model来导入使用。

```python
>>> from polls.models import Poll,Choice
>>> from django.utils import timezone
>>> p = Poll(question = "what's up?", pub_date = timezone.now())
>>> p
>>> p.save()
>>> Poll.objects.all()
[<Poll: Poll object>]
>>> p = Poll.objects.filter(question="what's up?")
>>> p
[<Poll: Poll object>]
>>> p.delete()
>>> Poll.objects.all()
[]
```

上面先生成一个Poll对象，然后保存到数据库，然后查找出来，再给删除了，相关的方法可以去手册里面看一下。

可是每次数据操作都返回一个Poll对象的话，没有什么可区别的，一般在定义数据模型的时候做些改变，也可以自定义一些方法。

```python
from django.db import models
from datetime import timedelta
from django.utils import timezone

# Create your models here.
class Poll(models.Model):
    question = models.CharField(max_length = 200)
    pub_date = models.DateTimeField('date published')

    def __unicode__(self):
        return self.question

    def was_published_recently(self):
        return self.pub_date >= timezone.now() - timedelta(days = 1)

class Choice(models.Model):
    poll = models.ForeignKey(Poll)
    choice = models.CharField(max_length = 200)
    votes = models.IntegerField()

    def __unicode__(self):
        return self.choice
```

修改之后要退出（CTRL+Z）shell，然后再次进入：

```python
>>> from polls.models import Poll,Choice
>>> from django.utils import timezone
>>> p = Poll(question = "what's up?", pub_date = timezone.now())
>>> p
<Poll: what's up?>
```

OK，今天就到这了，多多练习，Django手册在手什么都不怕了，不会的再去google一下，还有什么不能解决的呢？接下来要从后台搞起……
