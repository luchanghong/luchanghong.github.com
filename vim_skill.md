---
date: 2012-08-27 17:35:54 +08:00
title: vim小技巧集锦
layout: post
---
vim使用技巧集锦

1.不备份文件

?
1
set nobackup
2.设置Tab缩进

?
1
2
3
4
set tabstop=4
set softtabstop=4
set shiftwidth=4
set noexpandtab / expandtab
其中tabstop表示一个 tab 显示出来是多少个空格的长度，默认 8。

softtabstop表示在编辑模式的时候按退格键的时候退回缩进的长度，当使用expandtab时特别有用。

shiftwidth表示每一级缩进的长度，一般设置成跟softtabstop一样。

当设置成expandtab时，缩进用空格来表示，noexpandtab则是用制表符表示一个缩进。

3.行首和行尾

跟正则一块记忆：^ 表示开始，既行首；$表示结束，既行尾

另外：数字0和大写字母G也表示行首，快速按下2G则会跳到第二行的起始位置

更方便的操作：在命令行状态下输入行号（数字），然后回车就跳转过去了

4.翻页操作

GG：快速跳转到第一行

control+b：向上翻页 control+f：向下翻页

5.删除操作

dd：删除一行

6.缩进、复制、粘贴

按v进入visual状态，选择多行，’<’和’>’表示前后缩进

yy：复制当前行

选择多行后，y复制

p：粘贴

7.查找与替换

查找：/search words，按n键查找下一个，大写的N查找上一个

另外一个查找方法：?search words，从文档尾部开始查找，与上一种方法相反

替换：s/a/b 把a替换成b，替换当前行，%s/a/b则替换所有

———————————————————————————————————————————————————–
一图胜千言：



业精于勤而荒于嬉，行成于思而毁于随！熟方能生巧~~
