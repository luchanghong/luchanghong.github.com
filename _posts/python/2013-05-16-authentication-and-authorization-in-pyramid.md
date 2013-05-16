---
layout: post
title: Pyramid中的安全权限策略——细说Authentication和Authorization
category: python
tags: [python, pyramid, security, authentication, authorization]
description: 安全和权限策略是Pyramid中一个重要的组成部分，主要分为authentication（认证）和authorization（授权）两个部分。
---

## 安全策略

Pyramid提供一个可选的声明式的授权系统，当一个view被调用的时候，可以根据request里面的权限凭证和上下文去决定授权与否。下面看官方文档里关于工作流程的描述：

> - A request is generated when a user visits the application.
- Based on the request, a context resource is located through resource location. A context is located differently depending on whether the application uses traversal or URL dispatch, but a context is ultimately found in either case. See the URL Dispatch chapter for more information.
- A view callable is located by view lookup using the context as well as other attributes of the request.
- If an authentication policy is in effect, it is passed the request; it returns some number of principal identifiers.
- If an authorization policy is in effect and the view configuration associated with the view callable that was found has a permission associated with it, the authorization policy is passed the context, some number of principal identifiers returned by the authentication policy, and the permission associated with the view; it will allow or deny access.
- If the authorization policy allows access, the view callable is invoked.
- If the authorization policy denies access, the view callable is not invoked; instead the forbidden view is invoked.

Pyramid的安全策略明确的分为 **认证** 和 **授权** 两个部分。认证过程可以理解为包含在一个request中的权限凭证转换成一个或者多个principal标识符的机制。这些标识符实际上在request过程中代表用户和用户组。授权取决于这些标识符、被调用的view视图和上下文资源。

## 使用认证和授权策略

下面还是以`lbew`项目为例说明。

- 首先在__init__.py中添加一些配置：

    ```python
    # use authentication and authorization
    from pyramid.authentication import AuthTktAuthenticationPolicy
    from pyramid.authorization import ACLAuthorizationPolicy
    from lbew.security import groupfinder
    authn_policy = AuthTktAuthenticationPolicy('hereseekrit',
            callback = groupfinder, hashalg = 'sha512')
    authz_policy = ACLAuthorizationPolicy()

    config = Configurator(settings = settings, root_factory = 'lbew.resources.RootFactory',
            authentication_policy = authn_policy, authorization_policy = authz_policy)

    # 也可以用下面的配置方式
    config.set_authentication_policy(authn_policy)
    config.set_authorization_policy(authz_policy)
    ```

    AuthTktAuthenticationPolicy有很多参数，这里使用callback和hashalg，剩下的可以参考[官方文档][1]。

- 新建lbew/resources.py，然后code：

    ```python
    __author__ = 'luchanghong'
    #!/usr/bin/env python
    #-*- coding: utf8 -*-

    from pyramid.security import Allow, Deny, Everyone

    class RootFactory(object):
        __acl__ = [ (Allow, Everyone, 'view'),
                (Allow, 'group:viewers', 'view2'),
                (Allow, 'group:editors', 'edit') ]

        def __init__(self, request):
            pass
    ```

    在__init__.py中已经指定`root_factory = 'lbew.resources.RootFactory'`，否则是系统[default root_factory][2]。

- 新建lbew/security.py，然后code：

    ```python
    __author__ = 'luchanghong'
    #!/usr/bin/env python
    #-*- coding: utf8 -*-

    USERS = {'editor':'editor', 'viewer':'viewer'}
    GROUPS = {'editor':['group:editors'],
            'viewer':['group:viewers']}

    def groupfinder(userid, request):
        if userid in USERS:
            return GROUPS.get(userid, [])
    ```

    这就是为了callback使用，如果没有callback函数，那么principal只能是everyone。

[1]: http://docs.pylonsproject.org/projects/pyramid/en/1.4-branch/api/authentication.html#module-pyramid.authentication
[2]: http://docs.pylonsproject.org/projects/pyramid/en/1.4-branch/api/config.html#pyramid.config.Configurator.set_root_factory

没有接触过Pyramid的朋友可能在这一块很迷茫，事已至此，我们主要解决两个问题：

- view视图如何设置上权限？

- 授权到底是怎样的过程？

在这之前还要了解一下ACL和ACE的概念。 ACE（Access Control Entry）是单一的权限控制入口，这个元组一般组成形式是（允许/拒绝，用户/用户组，操作/（操作1，操作2，……））；而ACL（Access Control List）就是多个ACE组成的一个列表。下面是官方文档中的一个例子：

```python
from pyramid.security import Everyone
from pyramid.security import Allow

__acl__ = [
        (Allow, Everyone, 'view'),
        (Allow, 'group:editors', 'add'),
        (Allow, 'group:editors', 'edit'),
        ]
```

更多的资料参考[官方文档][3]。

[3]: http://docs.pylonsproject.org/projects/pyramid/en/1.4-branch/narr/security.html#elements-of-an-acl

## 为view视图添加permission

首先打开授权调试工具，在配置文件development.ini中把`pyramid.debug_authorization`设为true（line 10）：

```ini
[app:main]
use = egg:lbew

pyramid.reload_templates = true
pyramid.debug_authorization = true
```

下面的例子以之前[项目][4]为例。打开lbew/views/account.py，然后code：

```python
__author__ = 'luchanghong'
#!/usr/bin/env python
#-*- coding: utf8 -*-

from pyramid.view import view_config

class Account(object):
    def __init__(self, context, request):
        self.request = request
        self.context = context

    # everyone can visit
    @view_config(route_name = 'signup', renderer = 'string')
    def signup(self):
        print self.context
        return 'welcome to signup.'
```

打开浏览器访问`http://0.0.0.0:6543/account/signup`看到相应输出，然后看一下调试信息：

```bash
Starting server in PID 16407.
serving on http://0.0.0.0:6543
2013-05-16 17:49:48,495 DEBUG [lbew][Dummy-2] debug_authorization of url http://0.0.0.0:6543/account/signup (view name u'' against context <lbew.resources.RootFactory object at 0x10ee8f150>): Allowed (no permission registered)
<lbew.resources.RootFactory object at 0x10ee8f150>
```

从调试信息可以看到现在没有permission注册，而且context关联的是在__init__.py中设置的`lbew.resources.RootFactory`，否则就是默认的`<pyramid.traversal.DefaultRootFactory instance at 0x10c8cb3f8>`。

这里我们就来解决上面说的第一个问题：view和factory关联以及设置permission。在配置route的时候可以指定使用的factory，然后找到context：

```python
# lbew/routes.py
config.add_route(name = 'signup',   pattern = '/account/signup',    factory = 'lbew.resources.RootFactory')
```

指定factory参数之后，就会忽略__init__.py里root_factory的设置。然后为view添加permission：

```python
@view_config(route_name = 'signup', renderer = 'string', permission = 'view')
def signup(self):
    print self.context
    return 'welcome to signup.'
```

此时重启pserve，刷新上个页面看一下调试信息（先不管favicon.ico相关信息）：

```bash
2013-05-16 18:19:43,033 DEBUG [lbew][Dummy-4] debug_authorization of url http://0.0.0.0:6543/account/signup 
(view name u'' against context <lbew.resources.RootFactory object at 0x102241ed0>): ACLAllowed permission 'view' 
via ACE ('Allow', 'system.Everyone', 'view') in ACL [('Allow', 'system.Everyone', 'view'), ('Allow', 'group:viewers', 'view2'), 
('Allow', 'group:editors', 'edit')] on context <lbew.resources.RootFactory object at 0x102241ed0> for principals ['system.Everyone']
<lbew.resources.RootFactory object at 0x102241ed0>
```

可以看出此时用户的principals是`['system.Everyone']`，而RootFactory中允许`view`操作的是`('Allow', 'system.Everyone', 'view')`，所以可以成功授权。为了验证，我们做个修改：

```python
# lbew/resources.py
class RootFactory(object):
    __acl__ = [ (Allow, 'luchanghong', 'view'),
            (Allow, 'group:viewers', 'view2'),
            (Allow, 'group:editors', 'edit') ]

    def __init__(self, request):
        pass
```

再次调试会发现出现403 forbidden，调试信息：

```bash
2013-05-16 18:27:06,604 DEBUG [lbew][Dummy-3] debug_authorization of url http://0.0.0.0:6543/account/signup 
(view name u'' against context <lbew.resources.RootFactory object at 0x11012ea90>): ACLDenied permission 'view' 
via ACE '<default deny>' in ACL [('Allow', 'luchanghong', 'view'), ('Allow', 'group:viewers', 'view2'), 
('Allow', 'group:editors', 'edit')] on context <lbew.resources.RootFactory object at 0x11012ea90> for principals ['system.Everyone']
```

由于我设置只有`luchanghong`才能有`view`的权限，所以目前无权限访问。

## 总结

这一块涉及的比较多，所以写的时候感觉无从下手，关键是要理解。现在解决第二个问题，个人总结如下：

                                                     根据route指定的factory去获取context以及ACL 
                                                     |
                     根据request的url找到对应的route                                                如果没有permission则意味着没有权限限制，任何人都能访问
                     |                               |                                            |
                     |                               根据route_name找到对应的view视图并获取permission
     浏览器发起request                                                                             |
                     |                                                                             如果有permission则根据此 + 当前的principals + ACL去授权
                     |                        |默认的principals是system.Everyone
                     根据request获取principals
                                              |通过登陆获取userid以及user group，增加principals的值


有时间来写一下如何通过userid获取user一些信息并添加到principals，以及用户退出后重置principals。

[4]: http://luchanghong.com/python/2013/05/13/extend-your-pyramid-project.html "扩展你的Pyramid项目"
