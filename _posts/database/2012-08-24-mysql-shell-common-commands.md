--- 
wordpress_id: 519
wordpress_url: http://luchanghong.com/rosemary/?p=519
date: 2012-08-24 11:32:45 +08:00
layout: post
title: !binary |
  bXlzcWwgc2hlbGzluLjnlKjkuIDkupvlkb3ku6Q=

---
总结一下简单常用、实用的mysql shell命令，长时间不用难免会忘记，以备不时只需。

声明：关键字大写，库名、表名、字段名加尖点号（`），养成规范的习惯，我偷懒就不写了^_^!

<pre class="prettyprint">mysql -u user -p --default-character-set utf8</pre>

这个命令应该都会，但是如果你查询的时候出现乱码(针对中文)，那么就要设定一下默认字符格式了。<span style="text-decoration: underline; color: #ff0000;">如果还是乱码就要看一下你的terminal编码设置以及服务器的编码设置了。</span>

<pre class="prettyprint">desc table_nam;</pre>或者<pre class="prettyprint">show create table table_name;</pre>

这两个命令都会显示每个字段的格式，前者以表格形式展现，后者纯粹的sql语句。

<pre class="prettyprint">select * from table_name \G;</pre>

这个命令的重点是“\G”，如果query出来的每条记录长度超过屏幕宽度，显示的结果不适合阅读，而“\G"是让结果垂直显示，就比较方便查阅了。

<pre class="prettyprint">source file.sql</pre>

执行一个外部的.sql文件，在数据导入导出中常用。

&nbsp;

PS：不断更新……
