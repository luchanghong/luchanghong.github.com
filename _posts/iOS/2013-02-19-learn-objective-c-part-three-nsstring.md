---
layout: post
category: iOS
tags: [objective-c, iOS]
title: NSString——Objective-C学习（3）
description: 字符串是常用的数据类型，也是其他的复杂类型数据的基本组成单位，详细看一下字符串NSString的相关操作。
---

## 声明

 * 硬件：MacBook Pro 13.3寸 2012版
 * 系统：Mac OS X Mountain Lion 10.8.2
 * 软件：Xcode 4.6（昨天刚升级的）

## 练习

```objectivec
// declear a ns-string
NSString *str_1 = @"I am a ns-string";
NSString *str_2 = @"I am a ns-string";
NSString *str_3 = str_2;
NSString *my_string = [[NSString alloc] initWithFormat: @"hello world."];
NSLog(@"%@", my_string);

BOOL result_1 = str_1 == str_2;
BOOL result_2 = str_2 == str_3;
BOOL result_3 = str_3 == str_1;
NSLog(@"result_1 is %i, result_2 is %i, result_3 is %i", result_1, result_2, result_3);

NSString *str_4 = [[NSString alloc] init];
NSLog(@"the first position of str_4 is %p", str_4);
str_4 = str_3;

if (str_1 == str_4) {
    NSLog(@"now the position of str_4 is %p", str_4);
    
    // the position of each variable
    NSLog(@"the position of the variable str_1 is %p", str_1);
    NSLog(@"the position of the variable str_2 is %p", str_2);
    NSLog(@"the position of the variable str_3 is %p", str_3);
}

str_1 = @"changed";
NSLog(@"---------------------------");
NSLog(@"when chage str_1, the position is %p", str_1);
NSLog(@"str_2:%@, position:%p", str_2, str_2);


// compare the two different variables
BOOL result_4 = [str_1 isEqualToString: str_4];
BOOL result_5 = [str_4 isEqualToString: str_1];
if (result_4 && result_5) {
    NSLog(@"str_1 is equal to str4");
}

NSString *strNum1 = [NSString stringWithFormat: @"ss"];
NSString *strNum2 = [NSString stringWithFormat: @"gg"];
NSLog(@"%ld, %ld, %ld", [strNum1 compare: strNum2], [strNum2 compare: strNum1], [str_2 compare:str_3]);

// explode string
NSString *country = @"China,America,Japan,Canada";
NSArray *arr = [country componentsSeparatedByString: @","];
for (int i = 0; i < [arr count]; i++) {
    NSLog(@"%@", [arr objectAtIndex: i]);
}

// format string
int a = 10;
int b = 20;
NSString *str_5 = [NSString stringWithFormat: @"the sum of %d and %d is %d", a, b, a+b];
NSString *str_6 = [[NSString alloc] initWithString: str_5];
NSLog(@"str_5:%@, the position is %p", str_5, str_5);
NSLog(@"str_6:%@, the position is %p", str_6, str_6);

// search and replace in a string
NSString *myStr = @"I am a boy, and you?";
if ([myStr hasPrefix: @"I"]) {
    NSLog(@"The string '%@' has prefix 'I'.", myStr);
}
if ([myStr hasSuffix: @"you?"]) {
    NSLog(@"The string '%@' has suffix 'you?'.", myStr);
}

NSString *search_1 = @"I";
NSString *search_2 = @"a boy";
NSRange rang_1 = [myStr rangeOfString: search_1];
NSRange rang_2 = [myStr rangeOfString: search_2];
NSLog(@"The position of '%@' in string '%@' is %li", search_1, myStr, rang_1.location);
NSLog(@"The position of '%@' in string '%@' is %li", search_2, myStr, rang_2.location);

NSString *replace_res1 = [myStr stringByReplacingCharactersInRange: rang_1 withString: @"IIII"];
NSLog(@"%@", replace_res1);
NSString *replace_res2 = [myStr stringByReplacingOccurrencesOfString: @" " withString: @"|"];
NSLog(@"%@", replace_res2);

NSString *s1 = [NSString stringWithFormat: @"a"];
NSString *s2 = [NSString alloc];
NSLog(@"s1 position: %p, s2 position: %p", s1, s2);
s2 = s1;
NSLog(@"s1 position: %p, s2 position: %p", s1, s2);
s1 = [NSString stringWithFormat: @"b"];
NSLog(@"s1 = %@ position: %p, s2 = %@ position: %p", s1, s1, s2, s2);
```

对比输出结果：

```bash
2013-02-19 18:09:51.882 UseNSString[5556:303] hello world.
2013-02-19 18:09:51.884 UseNSString[5556:303] result_1 is 1, result_2 is 1, result_3 is 1
2013-02-19 18:09:51.885 UseNSString[5556:303] the first position of str_4 is 0x7fff75b2ec70
2013-02-19 18:09:51.885 UseNSString[5556:303] now the position of str_4 is 0x100002108
2013-02-19 18:09:51.885 UseNSString[5556:303] the position of the variable str_1 is 0x100002108
2013-02-19 18:09:51.886 UseNSString[5556:303] the position of the variable str_2 is 0x100002108
2013-02-19 18:09:51.886 UseNSString[5556:303] the position of the variable str_3 is 0x100002108
2013-02-19 18:09:51.886 UseNSString[5556:303] ---------------------------
2013-02-19 18:09:51.887 UseNSString[5556:303] when chage str_1, the position is 0x100002228
2013-02-19 18:09:51.887 UseNSString[5556:303] str_2:I am a ns-string, position:0x100002108
2013-02-19 18:09:51.888 UseNSString[5556:303] 1, -1, 0
2013-02-19 18:09:51.888 UseNSString[5556:303] China
2013-02-19 18:09:51.889 UseNSString[5556:303] America
2013-02-19 18:09:51.889 UseNSString[5556:303] Japan
2013-02-19 18:09:51.889 UseNSString[5556:303] Canada
2013-02-19 18:09:51.890 UseNSString[5556:303] str_5:the sum of 10 and 20 is 30, the position is 0x1006017e0
2013-02-19 18:09:51.890 UseNSString[5556:303] str_6:the sum of 10 and 20 is 30, the position is 0x1006017e0
2013-02-19 18:09:51.890 UseNSString[5556:303] The string 'I am a boy, and you?' has prefix 'I'.
2013-02-19 18:09:51.891 UseNSString[5556:303] The string 'I am a boy, and you?' has suffix 'you?'.
2013-02-19 18:09:51.891 UseNSString[5556:303] The position of 'I' in string 'I am a boy, and you?' is 0
2013-02-19 18:09:51.892 UseNSString[5556:303] The position of 'a boy' in string 'I am a boy, and you?' is 5
2013-02-19 18:09:51.892 UseNSString[5556:303] IIII am a boy, and you?
2013-02-19 18:09:51.893 UseNSString[5556:303] I|am|a|boy,|and|you?
2013-02-19 18:09:51.893 UseNSString[5556:303] s1 position: 0x7fff760ff3d0, s2 position: 0x10010a790
2013-02-19 18:09:51.894 UseNSString[5556:303] s1 position: 0x7fff760ff3d0, s2 position: 0x7fff760ff3d0
2013-02-19 18:09:51.894 UseNSString[5556:303] s1 = b position: 0x7fff761129f0, s2 = a position: 0x7fff760ff3d0
```

## String Format Specifiers

类似`%@`格式化请参考苹果官方文档：[String Format Specifiers](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html)
