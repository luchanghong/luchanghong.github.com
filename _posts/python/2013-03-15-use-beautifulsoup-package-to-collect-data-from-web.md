---
layout: post
title: 使用BeautifulSoup采集网页数据
tags: [python, beautifulsoup, web]
category: python
description: 因为我本人对正则表达式是比较反感的，能不用的时候一定会尽最大可能避免，所以在采集网页数据的时候我没用PHP，用的是Python，因为有BeautifulSoup。
---

## BeautigulSoup

> You didn't write that awful page. You're just trying to get some data out of it. Beautiful Soup is here to help. Since 2004, it's been saving programmers hours or days of work on quick-turnaround screen scraping projects.

上面的一段句话来自[官方][1]网页，看完之后就知道它的美妙了。

[1]: http://www.crummy.com/software/BeautifulSoup/ "BeautifulSoup"

## Install && Useage 

可以用`pip`或者`easy_install`安装在当前的`python`环境里，例如：

```bash
pip install beautifulsoup4

easy_install beautifulsoup4
```

使用的时候也很简单，首先导入：

```python
from BeautifulSoup import BeautifulSoup
```

具体用法可以参考下面一个DEMO。

## DEMO

这个DEMO很简单，实现内容是：使用`urllib2`获取网页信息，然后通过`BeautifulSoup`分析节点找出数据，最后是使用`MySQLdb`保存到数据库。

```python
#!/usr/bin/env python
#-*- coding: utf8 -*-

from urllib2 import urlopen,URLError
from BeautifulSoup import BeautifulSoup
from mysql.common import MySQLCurd
import MySQLdb

baseUrl = 'http://www.mingxing.com/zimu/'

def main():
    allStarDict = dict()
    print "Start..."

    for x in range(97, 123, 1):
        nameList = list()

        letter = chr(x)
        searchUrl = '%s%s' % ( baseUrl, letter )

        try:
            print "Get %s name-index stars..." % letter

            hd = urlopen(searchUrl, timeout = 60)
            content = hd.read()
            # turn gb2312 code to unicode
            # content = content.decode('gb2312')

            contentSoup = BeautifulSoup(content)
            nameListSoup = contentSoup.find('div', {'class': 'nx4'}).findAll('li')

            if len(nameListSoup) > 0:
                for star in nameListSoup:
                    starInfo = star.find('a')
                    starName = starInfo.string
                    starHref = starInfo['href']
                    nameList.append([starName, starHref])

                allStarDict[letter] = nameList
            print "Get %s name-index stars successfully, and the count is %d.\n" % (letter, len(nameList))

        except URLError, e:
            print e
            print "Get %s name-index stars failed.\n" % letter
            continue

    allStarList = list()
    for item in allStarDict.items():
        allStarList += item[1]

    print "There are %d stars we have got." % len(allStarList)
    print "---Endding---"
    return allStarDict, allStarList

if __name__ == '__main__':
    starData = main()

    # connect mysql database
    conn = MySQLdb.connect(
        host = '127.0.0.1',
        port = 3306,
        user = 'root',
        passwd = 'root',
        db = 'testdb',
        charset = 'utf8'
    )

    m = MySQLCurd(conn)
    insertData = list()
    for item in starData[0].items():
        index = item[0]
        for x in item[1]:
            insertData.append((x[0], x[1], index))

    sql = '''INSERT INTO `star`(`name`, `href`, `index`) VALUES (%s, %s, %s)'''
    m.executemany(sql, insertData)
```

**说明：**

-   代码中用到一个我自己封装的MySQL增删改查的一个类，提供[完整DEMO下载][DEMO]
-   关于`MySQLdb`的安装参考[Mac下安装MySQLdb][MySQLdb]

[DEMO]: /upload/attachement/20130315/BeautifulSoup_DEMO.tar.gz
[MySQLdb]: /database/2012/07/23/install-mysqldb-package-with-mac.html

