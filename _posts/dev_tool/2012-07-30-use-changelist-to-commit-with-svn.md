--- 
wordpress_id: 481
wordpress_url: http://luchanghong.com/rosemary/?p=481
date: 2012-07-30 14:06:07 +08:00
layout: post
title: svn用changelist提交
category: dev_tool
tags: [svn, changelist]
description: 在 svn 项目库里要提交一个已经完成的模块，而此时当前有一个正在开发的模块，显然前者要 commit ，而后者不需要，为了防止提交文件和路径出错，我们可以先做一个 changelist ，把待提交的文件添加到 changelist 里面，这样我们只提交 changelist 即可。
---
以前用习惯了windows下面的SVN，换到mac下要用命令了，虽然也有类似的GUI软件，但是总不能一直都要看图操作吧，更何况命令也不难，而且学会了以后有一种优越感，你懂的。

其实通过svn help、svn commit help等帮助命令就可以学会一些常用的操作。

今天项目提交的时候，有的一些文件是不需要提交的，毕竟本地和生产服务器上有些配置不同，所以就要一个个选择，这样有一个坏处就是不方便管理，有时候漏掉一两个文件是难免的。用changelist把要提交的文件分组过来，这样就容易多了，也方便管理。

## 一、创建一个changelist

可以通过命令

```bash
svn changelist help
```

来看一下用法，创建一个changelist并命名为res_update

```bash
svn changelist res_update test.txt
```

第一次创建就要添加一个文件才能成功，另外，changelist可简写cl

## 二、给changelist添加文件

这个命令其实和创建的一样的

```bash
svn changelist res_update test2.txt
```

## 三、删除changelist的文件

只是从changelist里面删除，并不是真正删除，确切说是移除吧

```bash
svn changelist --remove test.txt
```

## 四、提交changelist

把待更新的文件都添加到changelist之后，用commit提交

```bash
svn commit --changelist res_update
```

这样，就只提交changelist里面的文件了

说明：当remove一个changelist里所有文件之后这个changelist就消失了，提交之后也自然消失
