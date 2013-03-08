---
layout: post
category: php
tags: [php, rbac, codeigniter]
title: 使用CodeIgniter开发的一个基于RBAC的后台管理系统
description: 当开始开发一个项目，需求、数据库设计、页面设计等全部都OK的时候，我习惯于先从后台开发入手，这个时候就需要一个很实用而且方便的权限控制系统了，我的选择是RBAC这种设计模式。现在把简单的一个DEMO分享出来，如果有这个东西的话，几乎任何项目都可以在此基础上快捷高效的开发了。
---

## RBAC

Role-Based Access Control，指的是基于角色的访问控制。想要了解更多，可以看百度百科词条 [RBAC](http://baike.baidu.com/view/73432.htm) 。

## DEMO使用介绍

* 在本地配置一个虚拟域名指向DEMO目录，在`/application/config/config.php`中设定：
    
    $config['base_url'] = 'http://cifw.com'; //本地配置的虚拟域名

* 导入DEMO数据库，后面会给DEMO数据库下载

* `http://cifw.com/admin`直接登陆到后台，我在程序里没有做登陆验证，直接赋予管理员最高权限，相关代码在`application/controllers/admin/account.php`。

* 进入后台，有两个菜单：操作集合（可以理解为管理Menu）和角色管理。

**注意：由于在给角色分配权限的时候没有把主菜单对应的权限添加进来，所以没有子菜单（例如控制面板）的要添加一个不显示的子菜单。**

## DEMO下载

需要改进的地方还有很多，有见解的还请不吝赐教。

[DEMO源码下载](http://wwww.luchanghong.com/upload/attachement/20130308/RBAC_DEMO.tar.gz)
[DEMO数据库下载](http://wwww.luchanghong.com/upload/attachement/20130308/RBAC_DB.sql)
