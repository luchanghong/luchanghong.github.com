---
layout: post
title: 使用sed命令
category: linux_OSX
tags: [sed]
description: sed是一个很有用的命令，常用VIM编辑器就会有一种似曾相识的感觉，可以把sed理解为VIM的“在线版”。
---

## 关于sed

sed相当于一个编辑器，但是不用打开文件，批量操作更方便，命令类似vim。为了保证数据的安全，默认情况下sed并不直接操作源文件。

## 测试文件

```bash
lch@localhost:Desktop $ vim foo.txt
1 This is line 1.
1 This is line 1.
2 This is line 2.
3 This is line 3.
4 This is line 4.
5 This is line 5.
```

## 查找

```bash
# 查找第N行并输出
lch@localhost:Desktop $ sed -n '2p' foo.txt
This is line 2.
# 查找第N到M行并输出
lch@localhost:Desktop $ sed -n '1,2p' foo.txt
This is line 1.
This is line 2.
# 查找第N行至最后一行并输出
lch@localhost:Desktop $ sed -n '3,$p' foo.txt
This is line 3.
This is line 4.
This is line 5.
# 查找包含特定字符串的行并输出
lch@localhost:Desktop $ sed -n '/line 1/p' foo.txt
This is line 1.
# 从第N行开始查找含有特定字符串的行并输出
lch@localhost:Desktop $ sed -n '3, /line/p' foo.txt
This is line 3.
This is line 4.
# 查找分别包含两个特定字符之间的行并输出
lch@localhost:Desktop $ sed -n '/line 2/, /line 4/p' foo.txt
This is line 2.
This is line 3.
This is line 4.
```

Tips：`-n`参数会把发生改变的行输出，但要结合`/p`使用。 

## 替换

```bash
# 用字符串B替换字符串A
lch@localhost:Desktop $ sed 's/This/That/' foo.txt
That is line 1.
That is line 2.
That is line 3.
That is line 4.
That is line 5.
# 默认只会替换一行中第一个匹配的字符
lch@localhost:Desktop $ sed 's/i/ii/' foo.txt
Thiis is line 1.
Thiis is line 2.
Thiis is line 3.
Thiis is line 4.
Thiis is line 5.
# 加上g则会替换所有
lch@localhost:Desktop $ sed 's/i/ii/g' foo.txt
Thiis iis liine 1.
Thiis iis liine 2.
Thiis iis liine 3.
Thiis iis liine 4.
Thiis iis liine 5.
# 在匹配字符串后追加
lch@localhost:Desktop $ sed 's/2/&b/' foo.txt
This is line 1.
This is line 2b.
This is line 3.
This is line 4.
This is line 5.
# 在匹配两个字符串之间的行查找并替换
lch@localhost:Desktop $ sed -n '/line 2/, /line 4/ s/i/I/gp' foo.txt
ThIs Is lIne 2.
ThIs Is lIne 3.
ThIs Is lIne 4.
```

## 删除

```bash
# 删除某一行
lch@localhost:Desktop $ sed '2d' foo.txt
This is line 1.
This is line 3.
This is line 4.
This is line 5.
# 从某一行开始删除到末尾
lch@localhost:Desktop $ sed '3,$d' foo.txt
This is line 1.
This is line 2.
# 删除最后一行
lch@localhost:Desktop $ sed '$d' foo.txt
This is line 1.
This is line 2.
This is line 3.
This is line 4.
# 删除包含某一个字符串的一行（正则）
lch@localhost:Desktop $ sed '/4/d' foo.txt
This is line 1.
This is line 2.
This is line 3.
This is line 5.
```

## 多点编辑

```bash
# 使用 -e 参数，进行多次编辑操作
# -e 也等价于 --expression
lch@localhost:Desktop $ sed -n -e '1,2d' -e 's/i/I/gp' foo.txt
ThIs Is lIne 3.
ThIs Is lIne 4.
ThIs Is lIne 5.
```

## 读&写

```bash
# 把一个文件内容追加在第N行后面
lch@localhost:Desktop $ sed '1r read_file' foo.txt
This is line 1.
I am a reading file.
This is line 2.
This is line 3.
This is line 4.
This is line 5.
# 直接追加的字符（追加在目标行的下一行开头）
lch@localhost:Desktop $ sed "1 a\\
> i love you." foo.txt
This is line 1.
i love you.This is line 2.
This is line 3.
This is line 4.
This is line 5.
# 直接追加字符串（追加在目标行的开头）
lch@localhost:Desktop $ sed "/line/ i\\
i love you." foo.txt
i love you.This is line 1.
i love you.This is line 2.
i love you.This is line 3.
i love you.This is line 4.
i love you.This is line 5.
# 把第N行写到某个文件中去
lch@localhost:Desktop $ sed -n '1w write_file' foo.txt
lch@localhost:Desktop $ vim write_file
  1 This is line 1.
# 匹配写入
lch@localhost:Desktop $ sed -n '/line 1/, /line 3/w write_file' foo.txt
lch@localhost:Desktop $ vim write_file
  1 This is line 1.
  2 This is line 2.
  3 This is line 3.
```

## 说明

上面只是一些简单的例子，罗列的并不全面，要学会扩展，比如：

```bash
# 匹配两个字符串之间的行并做替换
lch@localhost:Desktop $ sed -n '/line 1/, /line 3/ s/i/I/gp' foo.txt
ThIs Is lIne 1.
ThIs Is lIne 2.
ThIs Is lIne 3.
# 还可以匹配追加
lch@localhost:Desktop $ sed '/line/r read_file' foo.txt
This is line 1.
I am a reading file.
This is line 2.
I am a reading file.
This is line 3.
I am a reading file.
This is line 4.
I am a reading file.
This is line 5.
I am a reading file.
lch@localhost:Desktop $ sed "/line 4/a\\
> i love you." foo.txt
This is line 1.
This is line 2.
This is line 3.
This is line 4.
i love you.This is line 5.
```

熟能生巧，多用用。
