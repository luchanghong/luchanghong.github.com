---
layout: post
title: MySQL事务回滚
tags: [mysql, transaction]
category: database
description: 前一段时间看到过关于事务管理方面的东西，今天恰巧遇到 MySQL 事务回滚这个话题，来简单实现一下 PHP 和 Python 操作 MySQL 数据库的时候的事务回滚。
---

## 什么是事物？事务回滚？

数据库事务(Database Transaction) ，是指作为单个逻辑工作单元执行的一系列操作。 事务处理可以确保除非事务性单元内的所有操作都成功完成，否则不会永久更新面向数据的资源。

事务回滚是指将该事务已经完成的对数据库的更新操作撤销。更多信息可以参考百科。

**注意：**常用的 MyISAM 类型数据库不支持事务，InnoDB 支持事务。

## 什么情况下使用事务以及事务回滚？

我想最常见的就是电子商城系统了，当一个订单被支付的时候，要完成商品库存减少、用户购物记录增加、用户交易记录增加、用户金额减少，或者网银交易记录增加等一系列动作，而每一个动作都对应一个表的操作（理解为一个事务），一旦中途出现错误，不管是人为的还是系统逻辑错误，甚至自然灾害影响都会导致最终数据不完整，通常这个时候我们使用事务管理，并在有异常的情况下执行事务回滚。

## PHP实现MySQL事务回滚

创建一个测试的数据库：

    mysql> CREATE DATABASE `shop_test` DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci;
    Query OK, 1 row affected (0.00 sec)

    mysql> use shop_test;
    Database changed

    mysql> CREATE TABLE IF NOT EXISTS `user_account`(
        -> `user` varchar(20) NOT NULL,
        -> `money` INT(10) NOT NULL,
        -> PRIMARY KEY (`user`)
        -> ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci;
    Query OK, 0 rows affected (0.51 sec)

    mysql> CREATE TABLE IF NOT EXISTS `user_order`(
        -> `id` INT(10) NOT NULL,
        -> `user` VARCHAR(20) NOT NULL,
        -> `price` INT(10) NOT NULL,
        -> `count` INT(10) NOT NULL,
        -> PRIMARY KEY (`id`)
        -> ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci AUTO_INCREMENT=1;
    Query OK, 0 rows affected (0.33 sec)

    mysql> INSERT INTO `user_account` VALUES ('luchanghong', '100');
    Query OK, 1 row affected (0.00 sec) 

PHP测试代码：
<pre class="prettyprint">
$conn = mysql_connect('127.0.0.1', 'root', 'root');
mysql_select_db('shop_test');
mysql_query('SET NAMES UTF8');

# start transaction
mysql_query("START TRANSACTION");
$sql = "INSERT INTO `user_order` VALUES ('1', 'luchanghong', '10', '2')";
mysql_query($sql);
$sql_2 = "UPDATE `user_account` SET `money` = `money` - 10*2 WHERE `user` = 'luchanghong'";
mysql_query($sql_2);

if (mysql_errno()){
    echo "error";
    mysql_query("ROLLBACK");
}else{
    echo "OK";
    mysql_query("COMMIT");
}
</pre>
    
执行一次后查看数据库：

    mysql> SELECT * FROM `user_account`;
    +-------------+-------+
    | user        | money |
    +-------------+-------+
    | luchanghong |    80 |
    +-------------+-------+
    1 row in set (0.00 sec)

    mysql> SELECT * FROM `user_order`;
    +----+-------------+-------+-------+
    | id | user        | price | count |
    +----+-------------+-------+-------+
    |  1 | luchanghong |    10 |     2 |
    +----+-------------+-------+-------+
    1 row in set (0.00 sec)

那么，我添加一个条件，就是每次更新完 `user_account` 表后检查用户的 money 是否为负值，如果为负值那么就要撤销之前的操作，执行事务回滚。

<pre class="prettyprint">
$conn = mysql_connect('127.0.0.1', 'root', 'root');
mysql_select_db('shop_test');
mysql_query('SET NAMES UTF8');

// start transaction
mysql_query("START TRANSACTION");
$sql = "INSERT INTO `user_order`(`user`, `price`, `count`) VALUES ('luchanghong', '10', '2')";
mysql_query($sql);
$sql_2 = "UPDATE `user_account` SET `money` = `money` - 10*2 WHERE `user` = 'luchanghong'";
mysql_query($sql_2);

if (mysql_errno()){
    echo "error \n";
    mysql_query("ROLLBACK");
}else{
    $money = check_remain_money('luchanghong');
    echo $money." ";
    if ($money < 0){ 
        echo "No enough money \n";
        mysql_query("ROLLBACK");
    }else{
        echo "OK \n";
        mysql_query("COMMIT");
    }   
}

function check_remain_money($user){
    $sql = "SELECT `money` FROM `user_account` WHERE `user` = '{$user}'";
    $result = mysql_fetch_assoc( mysql_query($sql) );
    return !empty($result) ? $result['money'] : 0;
}
</pre>

接着，在shell下多次执行这php文件（WIN下就手动执行几次吧），例如：
<pre class="prettyprint">
lch@LCH:~/Desktop $ for x in `seq 6`; do php transaction.php ; done
60 OK 
40 OK 
20 OK 
0 OK 
-20 No enough money 
-20 No enough money
</pre>

再看数据库数据：
    mysql> SELECT * FROM `user_account`;
    +-------------+-------+
    | user        | money |
    +-------------+-------+
    | luchanghong |     0 |
    +-------------+-------+
    1 row in set (0.00 sec)

    mysql> SELECT * FROM `user_order`;
    +----+-------------+-------+-------+
    | id | user        | price | count |
    +----+-------------+-------+-------+
    |  1 | luchanghong |    10 |     2 |
    |  2 | luchanghong |    10 |     2 |
    |  3 | luchanghong |    10 |     2 |
    |  4 | luchanghong |    10 |     2 |
    |  5 | luchanghong |    10 |     2 |
    +----+-------------+-------+-------+
    5 rows in set (0.00 sec)
    

## python下的实现

这里我用的是 [MySQLdb][] 这个包，例子如下：
<pre class="prettyprint">
#!/usr/bin/env python
#-*- coding: utf8 -*-

import MySQLdb

conn = MySQLdb.connect(host = '127.0.0.1', port = 3306, user = 'root',
        passwd = 'root', db = 'shop_test', charset = 'utf8')
cursor = conn.cursor()
user = 'luchanghong'

def main():
    # start transaction
    cursor.execute("START TRANSACTION")
    cursor.execute("INSERT INTO `user_order`(`user`, `price`, `count`) VALUES ('%s', '10', '2')" % user)
    cursor.execute("UPDATE `user_account` SET `money` = `money` - 10*2 WHERE `user` = '%s'" % user)
    money = check_remain_money(user)
    print money
    if int(money) < 0:
        cursor.execute("ROLLBACK")
        print "No enough money"
    else:
        cursor.execute("COMMIT")
        print "OK"

def check_remain_money(user):
    cursor.execute("SELECT `money` FROM `user_account` WHERE `user` = '%s'" % user)
    result = cursor.fetchone()
    return result[0] if result else 0

if __name__ == '__main__':
    main()
</pre>

再把用户的 money 改为 50 ：
    mysql> UPDATE `user_account` SET `money` = '50' WHERE `user` = 'luchanghong';
想测试PHP那样测试：
<pre class="prettyprint">
(env_push)lch@LCH:~/Desktop $ for x in `seq 3`; do python transaction.py; done
30
OK
10
OK
-10
No enough money
</pre>
看数据库结果：
    mysql> SELECT * FROM `user_account`;
    +-------------+-------+
    | user        | money |
    +-------------+-------+
    | luchanghong |    10 |
    +-------------+-------+
    1 row in set (0.00 sec)

    mysql> SELECT * FROM `user_order`;
    +----+-------------+-------+-------+
    | id | user        | price | count |
    +----+-------------+-------+-------+
    |  1 | luchanghong |    10 |     2 |
    |  2 | luchanghong |    10 |     2 |
    |  3 | luchanghong |    10 |     2 |
    |  4 | luchanghong |    10 |     2 |
    |  5 | luchanghong |    10 |     2 |
    |  6 | luchanghong |    10 |     2 |
    |  7 | luchanghong |    10 |     2 |
    +----+-------------+-------+-------+
    7 rows in set (0.00 sec)

OK，到此结束！

[MySQLdb]: http://luchanghong.com/database/2012/07/23/install-mysqldb-package-with-mac.html "MySQLdb"
