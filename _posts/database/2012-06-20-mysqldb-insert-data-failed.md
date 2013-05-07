--- 
wordpress_id: 323
wordpress_url: http://luchanghong.com/rosemary/?p=323
date: 2012-06-20 23:46:34 +08:00
layout: post
title: MySQLdb插入数据失败？insert data failed using MySQLdb？
category: database
tags: [mysql, MySQLdb]
description: 当 MySQLdb 遇上 innodb 类型的数据库，在做 insert 的时候，必须 commit 才能插入到数据表里面去。
---
天气热，头脑涨，效率不高。这不，今天下午一个很小的问题困扰我半天。

现在项目用的是mySQL数据库，起初，我选择了pymysql作为驱动。后来网上浏览发现用MySQLdb还是蛮多的，于是就换了，pymysql也好久没人维护了。说说问题吧，就是执行insert的时候，总是失败，却没有抛出什么异常，而且还一直堵着mySQL。

在公司火急火燎的，估计都晕乎了，所以也没搞出来结果。下班在地铁用手机上网查一下，发现一个重要的点，就是在执行玩execute之后要commit一下才行。具体方法有三：

1、在sql语句后加一个COMMIT命令

```python
cursor.execute('insert into user (id, name) values (1, 'luchanghong);COMMIT;')
```

2、给数据库连接conn预定一个commit

```python
conn = MySQLdb.connect(host = 'localhost', user = 'root', passwd = 'root', db = 'test')
conn.autocommit(1)
```

3、在释放数据库连接之前commit

```python
conn = MySQLdb.connect()
cursor.execute('insert into ...')
cursor.close()
conn.commit()
conn.close()
```

PS：我用的是第三种方法，其他两个还没来得及验证，有兴趣的测试一下吧
