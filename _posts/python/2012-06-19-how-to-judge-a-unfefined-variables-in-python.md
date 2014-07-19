--- 
wordpress_id: 316
wordpress_url: http://luchanghong.com/rosemary/?p=316
date: 2012-06-19 12:37:09 +08:00
layout: post
title: PHP/Python中判断变量是否定义
category: python
tags: [python]
description: 在 python 中使用一个不存在的变量肯定会报错，所以我们要在引用一个可能不存在或者未定义的变量的时候要做一下判断。
---
在PHP中判断变量是否定义（存在）很简单:

```php
isset(a)
```

在Python中怎样判断呢？

一、肯定会想到print语句，直接输出（如果真的不存在就会报错），那么，这样：

```python
>>> try:
...     print b
... except Exception:
...     print 'b is not defined'
...
b is not defined
>>>
```

二、用dir()

dir()函数常用查看一个类的方法，他比help()的输出更加简洁。如果直接输出你就会发现他的妙处：

```python
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__']
>>> a = 1
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__', 'a']
>>> def f1():
...     print 'ok'
...
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__', 'a', 'f1']
>>>
```

怎样判断就很明显了吧，再接着看

三、用locals().keys()

```python
>>> locals()
{'a': 1, 'f1': , '__builtins__': , '__package__': None, '__name__': '__main__', '__doc__': None}
>>> locals().keys()
['a', 'f1', '__builtins__', '__package__', '__name__', '__doc__']
>>>
```

判断方法：

```python
if 'a' in dir():
```

或者

```python
if 'a' in locals().keys():
```
