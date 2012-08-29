--- 
wordpress_url: http://luchanghong.com/rosemary/?p=282
wordpress_id: 282
title: !binary |
  b3B0cGFyc2XorqnkvaDnmoTohJrmnKzmm7TngbXmtLs=

layout: post
date: 2012-06-08 17:29:51 +08:00
---
写一个系统监控的脚本，把log文件的路径写死了，因为写的时候就是不可变的，所以被批一顿~~~有些东西该用还是得用，不能偷懒！

一般来说，脚本不像通常的编程，不管c/s还是b/s，大部分变量都是由用户操作产生，而脚本写好了就固定了，想改变某些变量的值怎么办？难道还去改代码吗？如果这样实用性也太差劲了。

写一个python脚本，常用optparse来帮助我们传递参数。一个简单的例子：
<pre>[python]
from optparse import OptionParser
parser = OptionParser()
parser.add_option('-t', '--test', action = 'store', dest = 'test', default = 'TEST', help = 'It is a test')
options, arg = parser.parse_args()
print options.test
[/python]</pre>
<pre>说明：</pre>
<ol>
	<li>-t 和 --test算是简写，或者理解参数的标志</li>
	<li>action表示参数类型，另外还有store_false，store_true等</li>
	<li>dest就是储存的变量名，到时候可以调用，如options.test</li>
	<li>default是默认值</li>
	<li>help是提示说明</li>
</ol>
注意：action为store_false或者store_true的时候，就表明这个变量是True或者False，所以没有默认值，一般是成对出现的：
<pre>[python]
__author__ = 'liuxiaopeng'
from optparse import OptionParser
parser = OptionParser()
parser.add_option('-t', action = 'store_true', dest = 'test', help = 'It is a test')
parser.add_option('-f', action = 'store_false', dest = 'test', help = 'It is a test')
options, arg = parser.parse_args()
print options.test
[/python]</pre>
如果上面的代码保存到test.py，那么使用的时候直接python test.py -t 就行了，后面跟上参数也无效。调用脚本的时候用-h或者help就会打印出参数的说明和用法。

值得说明：在代码里字符串我们都习惯加引号，但是执行脚本的时候，如果传递一个参数-t 是一个字符串，切记不要加引号，否则你得到的结果会多一对引号，即使str()强制转换也不行。
