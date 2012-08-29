--- 
wordpress_url: http://luchanghong.com/rosemary/?p=183
wordpress_id: 183
title: !binary |
  cHlyYW1pZOS4reWvvOWHumV4Y2Vs6KGo5qC8

layout: post
date: 2012-05-10 20:25:59 +08:00
---
Python对excel的操作，有篇文章提到过，<a title="python对EXCEL表格操作—pyExcelerator" href="http://luchanghong.com/rosemary/?p=130">猛击这里</a>。在web里如何导出一份excel表格呢？我用的是pyramid框架。

第一步，当然是把数据组成一个excel，方法就在上面说的那篇文章里，不同的是最后保存的时候，不是直接保存一个.xls文件，而是借助 StringIO 这个模块，他可以把文件放在内存里操作，不用直接保存到磁盘上。

第二步，就是数据处理后我们返回一个Response，指定他的content_type。
<pre>[python]
from pyramid.config import Configurator
from pyramid.view import view_config
from pyramid.response import Response
import cStringIO as StringIO
from pyExcelerator import *

config = Configurator()
config.add_route(name = 'export.excel', pattern = '/export/excel')
@view_config(route_name = 'export.excel')
def export_excel():
    out_put = StringIO.StringIO()
    w = Workbook()
    ws = w.add_sheet('test')
    ws.write(0, 0, 'name')
    ws.write(1, 0, 'luchanghong')
    ws.save(out_put)

    return Response(body = out_put.getvalue(), content_type = 'application/x-xls;', content_disposition = 'attachment; filename = test.xls;')
[/python]</pre>
