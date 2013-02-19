---
layout: post
category: iOS
tags: [objective-c, iOS]
title: 数据类型——Objective-C学习（2）
description: 按照学习语言的一贯思路，简单看一下Objective-C的数据类型。
---

## 声明

 * 硬件：MacBook Pro 13.3寸 2012版
 * 系统：Mac OS X Mountain Lion 10.8.2
 * 软件：Xcode 4.6

## 练习

几乎所有语言都跑不掉这些数据类型：整形、浮点型、字符串、布尔型、对象等，在Objective-C里注意一下特殊的`id/nil`以及`YES/NO`。

<pre class="prettyprint">
int main(int argc, const char * argv[])
{

    @autoreleasepool {
        
        // define an integer number
        int intData = 24;
        NSLog(@"%d is an integer.", intData);
        NSLog(@"%d", (int)12345678901);
        NSLog(@"%d", (int)1234567890);
        
        // define a float number
        float floatData = 3.1415926;
        NSLog(@"3.1415926 default is %f", floatData);
        NSLog(@"3.1415926 formats as two decimal %.2f", floatData);
        NSLog(@"%e", floatData);
        
        // define a short integer
        short int shortIntData = 123456789;
        NSLog(@"%hi", shortIntData);
        NSLog(@"%hi", (short int)123456);
        NSLog(@"%hi", (short int)12345);
        
        // definr a long integer
        long long int longIntData = 1234567890123456789L;
        NSLog(@"%lli", longIntData);
        
        // define a string
        char *strData = "I am a string";
        NSLog(@"%s", strData);
        
        // use NSNumber
        NSNumber *myInt = [NSNumber numberWithInt: 23];
        NSLog(@"%@", myInt);
        NSLog(@"%d", [myInt intValue]+1);
        
        NSNumber *myChar = [NSNumber numberWithChar:38];
        NSLog(@"%@", myChar);
        NSLog(@"%hhd", [myChar charValue]);
        NSLog(@"%d", [myChar intValue]+1);
        
        // int and string
        NSInteger nsInt = 100;
        NSString *nsStr = [NSString stringWithFormat: @"%ld", nsInt];
        NSLog(@"%@", nsStr);
        NSLog(@"%d", [nsStr intValue]);

        
        // insert code here...
        NSLog(@"Hello, World!");
        
    }
    return 0;
}
</pre>

对比看一下输出结果：

    2013-02-18 17:41:08.856 DataType[17717:303] 24 is an integer.
    2013-02-18 17:41:08.858 DataType[17717:303] -539222987
    2013-02-18 17:41:08.859 DataType[17717:303] 1234567890
    2013-02-18 17:41:08.859 DataType[17717:303] 3.1415926 default is 3.141593
    2013-02-18 17:41:08.860 DataType[17717:303] 3.1415926 formats as two decimal 3.14
    2013-02-18 17:41:08.860 DataType[17717:303] 3.141593e+00
    2013-02-18 17:41:08.860 DataType[17717:303] -13035
    2013-02-18 17:41:08.861 DataType[17717:303] -7616
    2013-02-18 17:41:08.861 DataType[17717:303] 12345
    2013-02-18 17:41:08.862 DataType[17717:303] 1234567890123456789
    2013-02-18 17:41:08.862 DataType[17717:303] I am a string
    2013-02-18 17:41:08.863 DataType[17717:303] 23
    2013-02-18 17:41:08.863 DataType[17717:303] 24
    2013-02-18 17:41:08.863 DataType[17717:303] 38
    2013-02-18 17:41:08.864 DataType[17717:303] 38
    2013-02-18 17:41:08.864 DataType[17717:303] 39
    2013-02-18 17:41:08.865 DataType[17717:303] 100
    2013-02-18 17:41:08.865 DataType[17717:303] 100
    2013-02-18 17:41:08.865 DataType[17717:303] Hello, World!

## String Format Specifiers

类似`%@`格式化请参考苹果官方文档：[String Format Specifiers](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)
