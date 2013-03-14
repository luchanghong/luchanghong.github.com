---
layout: post
category: dev_tool
title: SVN忽略文件或文件夹
tags: [svn]
description: 2013年第一篇博客，关于SVN忽略一些文件或者文件夹操作的。
---

每个项目中的配置文件都有区别，在本地开发和线上生产，之前一直很懒，不想去忽略提交一些配置文件，只是在提交的时候排除掉。但是在项目上传部署的时候又必须小心，害怕覆盖线上的配置，今天就硬头皮看一下。

如果你使用的是WIN，那么可以在SVN右键菜单里设置，可以设置全局忽略条件或者单独忽略掉一个文件以及文件夹，下面是command-line下的操作。

## 使用`svn propset svn:ignore`

```bash
lch@localhost:kidulty_www $ svn st
M       application/config/database.php
M       application/config/config.php
?       upload/contribute
?       upload/avatar
?       upload/201211
?       upload/comment
?       upload/recruit
lch@localhost:kidulty_www $ cd upload/
lch@localhost:upload $ svn propset svn:ignore "contribute
> avatar
> 201211
> comment
> recruit
> " .
property 'svn:ignore' set on '.'
lch@localhost:upload $ svn propget svn:ignore .
contribute
avatar
201211
comment
recruit

lch@localhost:upload $ svn ci -m "Ignore some uploaded directories"
Sending        upload

Committed revision 3492.
lch@localhost:kidulty_www $ svn st
M       application/config/database.php
M       application/config/config.php
```

需要注意的是：

* upload这个文件夹必须在当前SVN版本控制内
* 需要忽略的这几个文件夹必须在SVN版本控制外，也就是带有`?`标记
* 忽略文件也是同样的操作，而且必须条件也是要在版本控制之外
* `svn propset`等命令默认是当前目录下操作，也就是`.`，当然也可以赋值其他路径，不过还是推荐在要忽略文件所在目录下操作

## 使用`svn propedit svn:ignore`

针对上面的情况，操作如下：

```bash
svn propedit svn:ignore upload/
```

就会弹出编辑界面，之前设置一下SVN默认使用的编辑器：

```ini
export SVN_EDITOR=/usr/bin/vim
```

为了方便就加入到`.bash_profile`里面去。

编辑忽略对象的时候注意：

* 可以加`*`通配符如`*.txt`等
* 如果是文件夹，就写文件夹的名字，强调**不能在文件夹末尾加斜杠**
* 每个文件或者文件夹都独占一行

**说明** ：要忽略的对象都要在版本控制之外才行，如果已经在版本控制之内的文件要忽略就先导出，然后再从SVN版本控制中删除，最后再执行上面的操作。
