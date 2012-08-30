--- 
wordpress_url: http://luchanghong.com/rosemary/?p=130
date: 2012-04-17 15:22:15 +08:00
layout: post
wordpress_id: 130
title: !binary |
  cHl0aG9u5a+5RVhDRUzooajmoLzmk43kvZzigJRweUV4Y2VsZXJhdG9y

---
由于公司要对数据库里面数据做统计，在WEB上实现又没有太大的必要，所以就直接写个Python脚本搞起。统计出来的数据保存到excel表格里，上网google一下，尝试pyExcelerator这个类。

首先，在本地安装这个类库：pip install pyExcelerator或者easy_install pyExcelerator。我主要用到它往表格写数据的方法。
<pre>[python]
from pyExcelerator import *
#create a work book
w = Workbook()
ws = w.add_sheet('user')

#create xls header
xls_header = ['userName', 'email', 'tel']
for x in range(0, 3):
    ws.write(0, x, xls_header[x])

#write content
ws.write(1, 0, 'admin')
ws.write(1, 1, 'admin@admin.com')
ws.write(1, 2, '18888888888')
w.save('test.xls')
[/python]</pre>
上面这个小例子，先创建一个work book，然后添加一张表格，往表格写数据用write()方法，前三个参数分别是行、列、值，写完之后保存就可以了。

读取表格内容，注意，中间有单元格没有值的话，是不读的。读取方法：

wr = parse_xls('test.xls')

可以打印出来看一下数据结构。

这个类库对于单元格的操作不是怎么好使，但是写数据、读数据还是很方便的。

还有一个常用的操作EXCEL的库—xlrd，可以参考这篇博客：<a href="http://blog.csdn.net/suofiya2008/article/details/5587386">http://blog.csdn.net/suofiya2008/article/details/5587386</a>

&nbsp;
