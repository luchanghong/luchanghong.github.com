--- 
wordpress_url: http://luchanghong.com/rosemary/?p=150
wordpress_id: 150
title: !binary |
  cHl0aG9u5Lit5Li657G76K6+572u5LiA5Liq5Y+v5Y+Y5Y+Y6YeP55qE5bGe
  5oCn

layout: post
date: 2012-04-28 15:38:46 +08:00
---
学过PHP的都知道PHP可变变量有时候会被用到。所谓可变变量，简单理解就是变量名是也是个变量，例如：

[php]

$a = 'b';
$b = 'test';
echo $$a;

[/php]

之前在群里面有人问道Python里面一个类的属性是可变的时候怎么处理。比如：self.name，name是值为NAME的变量（name = 'NAME'），直接用的话就是找name属性，肯定不是我们想要的结果，那么怎么办？

方法一：用eval()函数

[python]

&gt;&gt;&gt; import sys
&gt;&gt;&gt; name = 'argv'
&gt;&gt;&gt; eval('sys.'+name)
['']

[/python]

但是eval()危险系数比较高，一般不推荐使用

方法二：vars()函数

[python]

&gt;&gt;&gt; import sys
&gt;&gt;&gt; name = 'argv'
&gt;&gt;&gt; vars(sys)[name]
['']

[/python]
