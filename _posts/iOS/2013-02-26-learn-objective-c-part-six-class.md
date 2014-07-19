---
layout: post
category: iOS
tags: [objective-c, iOS]
title: Class——Objective-C学习（6）
description: 学习Objective-C中的class中属性和方法的定义和使用。
---

## 声明

 * 硬件：MacBook Pro 13.3寸 2012版
 * 系统：Mac OS X Mountain Lion 10.8.2
 * 软件：Xcode 4.6

## 结构

新建一个文件，快捷键`command+n`，选择`Cocoa Touch`里面的`Objective-C class`，`Next`选择下一步，`subclass`指的是继承哪个类，我们选择`NSObject`，然后起个名字，创建完成之后会自动生成一个.h文件和一个.m文件，前者可以理解头文件，属性和方法的声明，后者是属性和方法的调用以及实现。

**注意** Objective-C中常把方法讲做Message。

## .h头文件

```objectivec
@interface Person : NSObject
{
    int age;
    NSString *sex;
    NSString *name;
}

// define some messages(methods)
-(void) printPersnalInfor;
-(void) setAge: (int) myAge setSex: (NSString *) mySex;
-(void) setName: (NSString *) MyName;
@end
```

## .m文件

```objectivec
@implementation Person

-(void) printPersnalInfor
{
    NSLog(@"Hey, i am a %@. \n My name is %@ and i am %d years old.", sex, name, age);
}

-(void) setAge: (int) myAge setSex: (NSString *) mySex
{
    self->age = myAge;
    // age = myAge;
    sex = mySex;
}

-(void) setName: (NSString *) MyName
{
    name = MyName;
}

@end
```

## main.m中调用

```objectivec
// use person class
Person *person = [[Person alloc] init];
[person setAge:24 setSex: @"male"];
[person setName: @"luchanghong"];
[person printPersnalInfor];
```

输出：

```bash
2013-02-26 17:36:07.354 CreateClass[2435:303] Hey, i am a male. My name is luchanghong and i am 24 years old.
```

## DEMO参考

下载：[CreateClass.tar.gz](http://www.luchanghong.com/upload/attachement/20130226/CreateClass.tar.gz)
