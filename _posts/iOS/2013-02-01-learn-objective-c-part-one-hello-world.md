---
layout: post
category: iOS
tags: [objective-c, iOS]
title: Hello World——Objective-C学习（1）
description: 学了一段时间的Objective-C，整理一下笔记，一来温故知新，再者以飨类似我这样的初学者。第一节当然是最经典的Hello World！
---

## 声明

 * 硬件：MacBook Pro 13.3寸 2012版
 * 系统：Mac OS X Mountain Lion 10.8.2
 * 软件：Xcode 4.6（昨天刚升级的）

## 创建项目

第一次学习，对于我等小菜来说创建哪一种类型的项目做练习比较好是相当茫然的，简述一下步骤。

新建工程`command+shift+n`，在`OS X`这一栏选`Application`，点选`Command Line Tool`，然后`Next`，选择`Type`为`Foundation`，输入`Product Name`，然后`Next`，选择项目保存目录，最后`Create`完成。

**注意：**因Xcode的版本不同，界面可能有些差异。

## Hello World

创建好`HelloWorld`项目之后，自动写上了输出`Hello, World!`的例子，只需要编译`command+b`运行`command+r`就可以看到Log输出了。不妨做一个简单的加法运算：

<pre class="prettyprint">
int main(int argc, const char * argv[])
{
    @autoreleasepool {
        
        int num_1 = 24;
        int num_2 = 12;
        int sum = num_1 + num_2;
        NSLog(@"the sum of %i and %i is %i", num_1, num_2, sum);
        
        // insert code here...
        NSLog(@"Hello, World!");
        
    }
    return 0;
}
</pre>

`command+r`查看输出：
    
    2013-02-02 00:25:10.701 HelloWorld[12580:303] the sum of 24 and 12 is 36
    2013-02-02 00:25:10.703 HelloWorld[12580:303] Hello, World!
