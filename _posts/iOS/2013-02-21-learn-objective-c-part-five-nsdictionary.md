---
layout: post
category: iOS
tags: [objective-c, iOS]
title: NSDictionary——Objective-C学习（5）
description: 稍微复杂的数据类型中数组和字典算是最常用的了，本节学习NSDictionary的用法。
---

## 声明

 * 硬件：MacBook Pro 13.3寸 2012版
 * 系统：Mac OS X Mountain Lion 10.8.2
 * 软件：Xcode 4.6

## 练习

<pre class="prettyprint">
// define a dictionary
NSDictionary *myDict = [NSDictionary dictionaryWithObjectsAndKeys: @"luchanghong", @"name", @"24", @"age", @"male", @"gender", nil];

// count dict
NSLog(@"myDict counts: %ld", [myDict count]);

// traverse dict
NSEnumerator *dictEnumerator = [myDict keyEnumerator];
for (NSObject *obj in dictEnumerator) {
    NSLog(@"%@:%@", obj, [myDict objectForKey: obj]);
}

NSEnumerator *dictValuesEnumerator = [myDict objectEnumerator];
for (NSObject *valueObj in dictValuesEnumerator) {
    NSLog(@"%@", valueObj);
}

// define a mutable dictionary
NSMutableDictionary *myMutDict = [NSMutableDictionary dictionaryWithObject:@"1990" forKey:@"born"];

// add a item
[myMutDict setObject:@"xinyang" forKey:@"hometown"];
NSEnumerator *myMutDictKeyEnum = [myMutDict keyEnumerator];
for (NSObject *keyObj in myMutDictKeyEnum) {
    NSLog(@"%@:%@", keyObj, [myMutDict objectForKey: keyObj]);
}

// replace a item
NSLog(@"%@", [myMutDict objectForKey: @"born"]);
[myMutDict setObject: @"2000" forKey: @"born"];
NSLog(@"%@", [myMutDict objectForKey: @"born"]);

// remove item
[myMutDict removeObjectForKey: @"born"];
NSLog(@"%ld", [myMutDict count]);

[myMutDict removeAllObjects];
NSLog(@"%ld", [myMutDict count]);

// insert code here...
NSLog(@"Hello, World!");
</pre>

对比输出结果：

    2013-02-21 18:11:13.772 useDictionary[3381:303] myDict counts: 3
    2013-02-21 18:11:13.780 useDictionary[3381:303] name:luchanghong
    2013-02-21 18:11:13.780 useDictionary[3381:303] age:24
    2013-02-21 18:11:13.781 useDictionary[3381:303] gender:male
    2013-02-21 18:11:13.781 useDictionary[3381:303] luchanghong
    2013-02-21 18:11:13.782 useDictionary[3381:303] 24
    2013-02-21 18:11:13.782 useDictionary[3381:303] male
    2013-02-21 18:11:13.783 useDictionary[3381:303] born:1990
    2013-02-21 18:11:13.783 useDictionary[3381:303] hometown:xinyang
    2013-02-21 18:11:13.784 useDictionary[3381:303] 1990
    2013-02-21 18:11:13.784 useDictionary[3381:303] 2000
    2013-02-21 18:11:13.785 useDictionary[3381:303] 1
    2013-02-21 18:11:13.785 useDictionary[3381:303] 0
    2013-02-21 18:11:13.785 useDictionary[3381:303] Hello, World!


## 新版特性

性特性定义数组：

    NSArray *array = @{@"a":@"A", @"b":@"B"};

