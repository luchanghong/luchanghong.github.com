---
layout: post
title: redis client常用命令
category: server
tags: [redis]
description: 关于redis以及在redis-cli中常使用的几个命令。
---

## 启动redis

```bash
lch@localhost:Desktop $ redis-server /usr/local/etc/redis.conf
```

## 数据基本操作增删改查

- 关于redis里的db

    进入客户端，默认使用编号为0的数据库，可以通过命令切换，如：

    ```bash
    lch@localhost:Desktop $ redis-cli
    redis 127.0.0.1:6379> select 1
    OK
    redis 127.0.0.1:6379[1]> select 0
    OK
    redis 127.0.0.1:6379>
    ```

- 设置一对key-value

    ```bash
    redis 127.0.0.1:6379> set name luchanghong
    OK
    redis 127.0.0.1:6379> set gender male
    OK
    redis 127.0.0.1:6379> set age 24
    OK
    ```

- 匹配查找key

    ```bash
    redis 127.0.0.1:6379> keys *
    1) "age"
    2) "gender"
    3) "name"
    redis 127.0.0.1:6379> keys name
    1) "name"
    redis 127.0.0.1:6379> keys nam
    (empty list or set)
    redis 127.0.0.1:6379> keys nam*
    1) "name"
    ```

- 取出key对应的value

    ```bash
    redis 127.0.0.1:6379> get name
    "luchanghong"
    redis 127.0.0.1:6379> get gender
    "male"
    redis 127.0.0.1:6379> get age
    "24"
    ```

- 判断key是否存在

    ```bash
    redis 127.0.0.1:6379> exists name
    (integer) 1
    redis 127.0.0.1:6379> exists names
    (integer) 0
    ```

- 删除key

    ```bash
    redis 127.0.0.1:6379> set test1 1
    OK
    redis 127.0.0.1:6379> set test2 2
    OK
    redis 127.0.0.1:6379> del test1
    (integer) 1
    redis 127.0.0.1:6379> exists test1
    (integer) 0
    redis 127.0.0.1:6379> exists test2
    (integer) 1
    ```

- 设置/查询多个key

    ```bash
    redis 127.0.0.1:6379> mset passwd 123 city beijing
    OK
    redis 127.0.0.1:6379> mget passwd city
    1) "123"
    2) "beijing"
    ```

## list操作

```bash
redis 127.0.0.1:6379> lpush people lch
(integer) 1
redis 127.0.0.1:6379> lset people 0 luchanghong
OK
redis 127.0.0.1:6379> lpush people male
(integer) 2
redis 127.0.0.1:6379> llen people
(integer) 2

redis 127.0.0.1:6379> lrange people 0 1
1) "male"
2) "luchanghong"
redis 127.0.0.1:6379> lindex people 0
"male"
redis 127.0.0.1:6379> lindex people 1
"luchanghong"
```

## set操作

```bash
redis 127.0.0.1:6379> sadd myset a
(integer) 1
redis 127.0.0.1:6379> sadd myset b c
(integer) 2
redis 127.0.0.1:6379> smembers myset
1) "c"
2) "a"
3) "b"
redis 127.0.0.1:6379> sismember myset d
(integer) 0
redis 127.0.0.1:6379> sismember myset a
(integer) 1
```

更多的命令可以去官网查看：[点击访问][1]

[1]: http://www.redis.io/commands "Command reference – Redis"
