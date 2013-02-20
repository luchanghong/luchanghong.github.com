---
layout: post
category: iOS
tags: [objective-c, iOS]
title: NSArray——Objective-C学习（4）
description: 在Objective-C稍微复杂的数据类型中，数组和字典算是最常用的了，本节学习NSArray的用法。
---

## 声明

 * 硬件：MacBook Pro 13.3寸 2012版
 * 系统：Mac OS X Mountain Lion 10.8.2
 * 软件：Xcode 4.6

## 练习

<pre class="prettyprint">
 // define an array
NSArray *emptyArr = nil;
NSLog(@"%ld", [emptyArr count]);

NSArray *myArr = [NSArray arrayWithObjects: @"a", @"b", @"c", nil];
NSLog(@"%ld", [myArr count]);

// traverse array
for (NSObject *arrObj in myArr) {
    NSLog(@"I am %@", arrObj);
}
NSLog(@"-------------------------");

// get a value through index
NSLog(@"Index 2 is %@", [myArr objectAtIndex: 2]);

// define a mutable array
NSMutableArray *mutableArr = [NSMutableArray arrayWithObjects: @"a", @"b", @"1", @"2", nil];
for (NSObject *obj in mutableArr) {
    NSLog(@"%@", obj);
}
NSLog(@"-------------------------");

// add item
[mutableArr addObject: @"c"];
[mutableArr addObject: @"b"];
for (NSObject *obj in mutableArr) {
    NSLog(@"%@", obj);
}
NSLog(@"-------------------------");

// insert an item
[mutableArr insertObject: @"error" atIndex: 1];
for (NSObject *obj in mutableArr) {
    NSLog(@"%@", obj);
}
NSLog(@"-------------------------");


// remove an item
[mutableArr removeObject: @"1"];
[mutableArr removeObject: @"b"];
NSRange range = NSMakeRange(0, 2);
[mutableArr removeObjectsInRange: range];
for (NSObject *obj in mutableArr) {
    NSLog(@"%@", obj);
}
NSLog(@"-------------------------");

// replace an item
[mutableArr replaceObjectAtIndex: 1 withObject: @"right"];
for (NSObject *obj in mutableArr) {
    NSLog(@"%@", obj);
}
NSLog(@"-------------------------");

// use enumerator
NSEnumerator *enumeRator = [mutableArr objectEnumerator];
id object;
while (object = [enumeRator nextObject]) {
    NSLog(@"%@", object);
}
NSLog(@"-------------------------");

// insert code here...
NSLog(@"Hello, World!");
</pre>

对比输出结果：

    2013-02-20 18:41:08.273 UseArray[5215:303] 0
    2013-02-20 18:41:08.275 UseArray[5215:303] 3
    2013-02-20 18:41:08.276 UseArray[5215:303] I am a
    2013-02-20 18:41:08.276 UseArray[5215:303] I am b
    2013-02-20 18:41:08.277 UseArray[5215:303] I am c
    2013-02-20 18:41:08.277 UseArray[5215:303] -------------------------
    2013-02-20 18:41:08.277 UseArray[5215:303] Index 2 is c
    2013-02-20 18:41:08.278 UseArray[5215:303] a
    2013-02-20 18:41:08.278 UseArray[5215:303] b
    2013-02-20 18:41:08.279 UseArray[5215:303] 1
    2013-02-20 18:41:08.279 UseArray[5215:303] 2
    2013-02-20 18:41:08.280 UseArray[5215:303] -------------------------
    2013-02-20 18:41:08.280 UseArray[5215:303] a
    2013-02-20 18:41:08.281 UseArray[5215:303] b
    2013-02-20 18:41:08.281 UseArray[5215:303] 1
    2013-02-20 18:41:08.281 UseArray[5215:303] 2
    2013-02-20 18:41:08.282 UseArray[5215:303] c
    2013-02-20 18:41:08.282 UseArray[5215:303] b
    2013-02-20 18:41:08.283 UseArray[5215:303] -------------------------
    2013-02-20 18:41:08.283 UseArray[5215:303] a
    2013-02-20 18:41:08.284 UseArray[5215:303] error
    2013-02-20 18:41:08.284 UseArray[5215:303] b
    2013-02-20 18:41:08.284 UseArray[5215:303] 1
    2013-02-20 18:41:08.285 UseArray[5215:303] 2
    2013-02-20 18:41:08.286 UseArray[5215:303] c
    2013-02-20 18:41:08.287 UseArray[5215:303] b
    2013-02-20 18:41:08.288 UseArray[5215:303] -------------------------
    2013-02-20 18:41:08.289 UseArray[5215:303] 2
    2013-02-20 18:41:08.290 UseArray[5215:303] c
    2013-02-20 18:41:08.291 UseArray[5215:303] -------------------------
    2013-02-20 18:41:08.292 UseArray[5215:303] 2
    2013-02-20 18:41:08.292 UseArray[5215:303] right
    2013-02-20 18:41:08.293 UseArray[5215:303] -------------------------
    2013-02-20 18:41:08.298 UseArray[5215:303] 2
    2013-02-20 18:41:08.299 UseArray[5215:303] right
    2013-02-20 18:41:08.300 UseArray[5215:303] -------------------------
    2013-02-20 18:41:08.300 UseArray[5215:303] Hello, World!

## 新版特性

定义数组：

    NSArray *array = @[@"a", @"b"];

