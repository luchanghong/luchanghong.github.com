---
layout: post
category: iOS
tags: [objective-c, iOS]
title: Class Property——Objective-C学习（7）
description: 学习Objective-C中的class中Property属性的定义和使用。
---

## 声明

 * 硬件：MacBook Pro 13.3寸 2012版
 * 系统：Mac OS X Mountain Lion 10.8.2
 * 软件：Xcode 4.6

## setter method

在实例化之后，如果想要调用或者重新赋值给一个属性，按照正常逻辑要写两个函数：调用、赋值。为了代码的简单和易读，则会在这两个函数命名上做文章：

    属性名称：userName
    调用属性方法命名：userName
    赋值属性方法命名：setUserName
    实例化之后使用：NSString *name = instanceObj.userName; / instanceObj.userName = @'lch';

**注意：这里的setUserName就是一个setter method**

## property

使用`setter method`总是显得有点啰嗦，那么就引入`property`，有了它就不用写调用和赋值这两个函数了：

    @property (nonatomic, weak) NSString *userName;

对，就是这么简单！这里只是介绍`property`最基本和最简单的使用方法，更多的内容请移步官方文档。

## DEMO参考

下载：[CreateClass.tar.gz](http://www.luchanghong.com/upload/attachement/20130311/ClassProperty.tar.gz)
