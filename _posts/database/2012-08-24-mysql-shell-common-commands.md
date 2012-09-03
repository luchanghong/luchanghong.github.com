--- 
wordpress_id: 519
wordpress_url: http://luchanghong.com/rosemary/?p=519
date: 2012-08-24 11:32:45 +08:00
layout: post
title: !binary |
  bXlzcWwgc2hlbGzluLjnlKjkuIDkupvlkb3ku6Q=
category: database
tags: [mysql, shell]
description: Mysql Shell 在服务器端是很好用的 Mysql 管理工具，掌握一些常用的 shell command 会大大提高我们的工作效率。这里记录一些我在日常网站开发与维护中使用到的命令，会不断更新。
---
总结一下简单常用、实用的mysql shell命令，长时间不用难免会忘记，以备不时只需。

声明：关键字大写，库名、表名、字段名加尖点号（`），养成规范的习惯，我偷懒就不写了^_^!

- <pre class="prettyprint">mysql -u user -p --default-character-set utf8</pre>
这个命令应该都会，但是如果你查询的时候出现乱码(针对中文)，那么就要设定一下默认字符格式了。<span style="text-decoration: underline; color: #ff0000;">如果还是乱码就要看一下你的terminal编码设置以及服务器的编码设置了。</span>

- <pre class="prettyprint">desc table_nam;</pre>或者<pre class="prettyprint">show create table table_name;</pre>
这两个命令都会显示每个字段的格式，前者以表格形式展现，后者纯粹的sql语句。

- <pre class="prettyprint">select * from table_name \G;</pre>
这个命令的重点是“\G”，如果query出来的每条记录长度超过屏幕宽度，显示的结果不适合阅读，而“\G”是让结果垂直显示，就比较方便查阅了。

- <pre class="prettyprint">source file.sql</pre>
执行一个外部的.sql文件，在数据导入导出中常用。

- <pre class="prettyprint">select DATE_FORMAT(`date_column`, 'Y%-m%-d%') from table_name;</pre>
这是将表里面的日期字段格式化一下，这里格式化的是 `2012-09-01 12:00:00` 这样的日期

- <pre class="prettyprint">select FROME_UNIXTIME(`timestamp_column`, '%Y-%m-%d %H:%i:%S') from table_name;</pre>
这个是把 `NUIX` 时间戳格式化成年月日时分秒的格式

PS：不断更新……
