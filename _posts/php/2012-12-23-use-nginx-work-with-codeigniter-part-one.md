---
layout: post
title: Nginx配置CodeIgniter项目（一）
description: 最近一个项目用Nginx作为WEB服务器，在Nginx上部署CI项目的时候遇到一些问题，想必好多朋友都遇到或者已经解决了，特总结一下。
category: php
tag: [php,codeigniter]
---

**首先我使用的是Nginx的`URL rewrite`的方法。**

## 开启`enable_query_strings`

开启方法很简单：在`application/config/config.php`中设置：

    line 158: $config['enable_query_strings'] = FALSE;

在CI开发的项目中用的`PATH_INFO`模式，要在Nginx配置中重写URL，那么就要在CI的配置文件中开启字符串查询，对比URL形式的变化：

    URL：www.xxx.com/user/profile
    字符串查询模式：www.xxx.com/index.php?c=user&m=profile

把前台和后台的URL按照一定规则重写之后，测试都OK。但是在分页的时候出点问题，因为在开启字符串查询之后生成的分页URL地址有变化：

    未开启：/user/list/10
    开启字符串查询之后：/user/list&per_page=10

之所以会出现下面错误的URL是我在生成分页的时候，`base_url`格式没有变化，那么从`/user/list`变成对应的`/index.php?c=user&m=list`之后就会变成下面情况：

    第二页：www.xxx.com/index.php?c=user&m=list&per_page=10
    第三页：www.xxx.com/index.php?c=user&m=list&per_page=20
    第四页：www.xxx.com/index.php?c=user&m=list&per_page=30

而我设置的pagesize一直都是10，这样的话per_page应该一直是10的。去看一下Pagination这个类的代码，发现per_page只是`query_string_segment`的默认值，我误以为是`per_page`这个参数了。

## 分页兼容rewrite

总结一下分页这块如果要兼容rewrite的写法，那么就在生成分页的时候改变一下`base_url`参数：

    方法一：/index.php?c=user&m=list，结果是：/index.php?c=user&m=list&per_page=10
    方法二：/user/list?，结果是：/user/list?&per_page=10

分页的SQL就是：

```php
$this->db->limit($pagesize, $this->input->get('per_page'));
```

为了保持URL一致性还是使用第二种方法，后面也可以随便加个没用的参数让结果变成`/user/list?x=xxx&per_page=10`。

## 讨论

 * 关于分页。当然也可以不使用CI自带的分页，或者把Pagination.php改一改。
 * 如果是作为method的一个参数传递的时候，正常URL如：`/user/arg1/arg2/arg3`，那么在rewrite的时候就没法传递过去了（至少我还没找到解决的方法），要想解决除非把参数变成GET方式传递，这就要改动程序了，所以不推荐。
 * 而且使用rewrite就要针对不同形式的URL，如果项目很复杂的话就成了累赘，所以就寻求另外一种方法：让Nginx把PATH_INFO传递给fastcgi，见下一篇文章吧。
