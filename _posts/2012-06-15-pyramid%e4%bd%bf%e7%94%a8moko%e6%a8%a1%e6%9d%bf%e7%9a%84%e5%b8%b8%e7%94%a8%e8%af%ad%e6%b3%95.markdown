--- 
wordpress_url: http://luchanghong.com/rosemary/?p=302
wordpress_id: 302
title: !binary |
  cHlyYW1pZOS9v+eUqG1va2/mqKHmnb/nmoTluLjnlKjor63ms5U=

layout: post
date: 2012-06-15 15:22:58 +08:00
---
pyramid常用moko模板，在简单配置moko模板之后就可以使用了。常用的当然是HTMl模板，配制方法请看<a title="用pyramid创建一个完整的WEB Project" href="http://luchanghong.com/rosemary/?p=284">这篇文章</a>。下面是几个常用的模板语法的例子：

一、输出变量或表达式

[html]${my_name}
${a + b}
[/html]

二、字符串转义

[html]{string}[/html]：默认把html标签等特殊字符原意输出，如果想以html形式输出，这样写：[html]{string | n}[/html]

三、定义Python语句

[html]

&lt;%
a = 1
b = 2
%&gt;
${a + b}
&lt;% l = [x for x in 'test'] %&gt;

[/html]

四、高级别的python语句块

这里高级的意思是多行python的写法，比如定义一个函数
<pre>[html]
&lt;%!
    def get_set_value(arr, index = 0):
        set_list = [x for x in arr]
        return set_list[index]
%&gt;
${get_set_value(my_set)}
[/html]</pre>
五、流程控制for / if

以%开头，结尾%end：
<pre>[html]
% for x in range(9):
    ${x}
% endfor
[/html]</pre>
六、其他

文件包含，常用包含头文件[html]&lt;% include file='header.html' %&gt;[/html]

另外，还可以自己定义一下block块，详情去官网看一下稳定吧 <a href="http://docs.makotemplates.org/">http://docs.makotemplates.org</a>
