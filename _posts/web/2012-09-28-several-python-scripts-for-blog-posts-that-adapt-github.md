---
layout: post
title: 分享几个转换文章到github的python脚本
category: web
tags: [python, github]
description: 由于我之前自己用PHP写个博客，然后又用wordpress，好多文章不想舍弃，但是转换到github上又有点麻烦。今天整理一下自己写的几个文章转换的脚本，也许有些人会遇到此类问题，我这个只是参考，抛砖引玉了。
---

## wordpress转到github

这个用 Jekyll 自带的脚本就行，参考之前[一篇文章](http://luchanghong.com/web/2012/09/01/start-write-blog-on-github.html)。若果你对转换结果不满意可以自己在里面修改，但是wordpress的分类和tag搞得太麻烦，有点不好弄。

## 普通文章转到github

这是我之前自己写的博客系统，数据库也是自己定义的，转换的时候较为简单，下面代码仅供参考：

```python
#!/usr/bin/env python
#-*- encoding: utf8 -*-

import MySQLdb

conn = MySQLdb.connect('127.0.0.1', 'root', 'root', 'a1207232132', charset='utf8')
cursor = conn.cursor()
cursor.execute('select a.id,a.title,a.content,a.tag,FROM_UNIXTIME(a.post_time, \'%Y-%m-%d\'),b.sort_name from lch_article as a, lch_article_sort as b where a.sort_id = b.sort_id and a.s_id = 1')

articles = list()

for row in cursor.fetchall():
    tags = row[3].replace(' ', ', ')
    art = row[2]
    f = open('post/%s-%s.md' % (row[4], row[0]), 'w')
    content = '''---
layout: post
title: %s
category: %s
tags: [%s]
date: %s
---
%s
''' % (row[1], row[5], tags, row[4], art)
    f.write(content.encode('utf8'))
    f.close()
```

## 文件编码转换
如果 posts 的文件编码是 gbk 的，想转成 utf8 ，参考下面代码：

```python
#!/usr/bin python
# -*- coding: utf-8 -*-
import os
from optparse import OptionParser

def change_encoding(filepath, out_dir, dir_list = [], file_list = []):
    unexpect_file_list = ['.', '..', '.DS_Store', '.svn']
    files = [x for x in os.listdir(filepath) if x not in unexpect_file_list]
    for x in files:
        cur_file = os.sep.join([filepath, x]) 
        out_file = os.sep.join([out_dir,x])
        if os.path.isdir(cur_file):
            dir_list.append(cur_file)
            try:
                os.mkdir(out_file)
            except:
                print out_file+' is already exists!'
            change_encoding(cur_file, out_file)
        else:
            file_list.append(cur_file)
            with open(cur_file) as old_file:
                content = old_file.read()
                try:
                    content = content.decode('gbk').encode('utf8')
                except:
                    pass
                new_file = open(out_file, 'w+')
                new_file.write(content)
                new_file.close()
    return len(dir_list), len(file_list)

if __name__ == '__main__':
    #filepath = '/Users/lch/work/ygl/wild_china/luchanghong.wildchina/trunk/includes'
    #old_dir = os.path.basename(filepath)
    #out_dir = os.sep.join([os.getcwd(), old_dir])
    usage = "%<progs> [options]"
    parser = OptionParser(usage)
    parser.add_option('-f', '--filepath', action = 'store', dest = 'filepath', help = 'file path')
    parser.add_option('-o', '--outputpath', action = 'store', dest = 'outputpath', default = os.getcwd(),  help = 'out put file path')
    options, args = parser.parse_args()

    filepath = os.path.normpath(
        os.path.realpath(options.filepath)
    )
    dirname = os.path.basename(filepath)
    out_dir = os.path.normpath(
        os.path.realpath(
            os.sep.join([options.outputpath, dirname])
        )
    )

    try:
        os.mkdir(out_dir)
    except:
        print out_dir+' is already exists!'

    print change_encoding(filepath, out_dir)
```

## 404跳转

因为之前一些post的路径和现在不一致，而搜索引擎还保留之前的结果，所以从搜索引擎来的连接可能是原来的，那么就出现了404页面，于是做一个措施：把每个404对应的URL做一个redirect。比如 `http://luchanghong.com/ignorant/html/70.html` -> `http://luchanghong.com/php/2011/07/11/70.html`，参考代码：

```python
#!/usr/bin/env python
#-*- coding: utf8 -*-

import os

direct_dir = '/Users/lch/dev/luchanghong.github.com/ignorant/html/'
post_dir = '/Users/lch/dev/luchanghong.github.com/_posts/php/'

# create all html files
for post in os.listdir(post_dir):
    # get post info
    post_info = post.split('-')
    post_year = post_info[0]
    post_month = post_info[1]
    post_day = post_info[2]
    post_id = post_info[3][:-3]

    try:
        post_id = int(post_id)
        file_name = '%s%d.html' % (direct_dir, post_id)
        os.system('touch %s' % file_name)
    except:
        continue

    # get post category
    with open(post_dir+post, 'r') as post_content:
        post_lines = post_content.readlines()
        post_category = post_lines[3][9:].strip()

    content = ''' 
    <html>
        <header>
            <script type="text/javascript">
                function Go(){
                    window.location.href = 'http://luchanghong.com/%(category)s/%(year)s/%(month)s/%(day)s/%(id)d.html';
                }
            </script>
        </header>
        <body onload="Go()">
        </body>
    </html>
    ''' %{
            'category': post_category,
            'year': post_year,
            'month': post_month,
            'day': post_day,
            'id': post_id,
        }
    f = open(file_name, 'w')
    f.write(content)
    f.close()
```

觉得有用的拿去针对自己的情况修修补补吧~~莫见笑！
