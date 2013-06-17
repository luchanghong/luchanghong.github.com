---
layout: post
title: 学习使用awk
category: linux_OSX
tags: [awk]
description: 对于awk，平时工作中很少用到，毕竟我的专职工作还是程序开发，这个命令对于系统运维和架构来说应该是小菜一碟。刚开始接触的时候很好奇，看了一点很快又忘记了，恐怕还是用的不够多，这次做个总结，以便日后再用到的时候及时唤醒。
---

## 测试文件

```bash
lch@localhost:Desktop $ vim student.txt
1 name    age     sex     location
2 luchanghong     24      male    beijing
3 luchanghong_2     25      male   tianjing
4 luchanghong_3     26      male   shanghai
```

## 基本用法

- 按行输出

    ```bash
    lch@localhost:Desktop $ awk '{print}' student.txt
    name    age     sex     location
    luchanghong     24      male    beijing
    luchanghong_2     25      male   tianjing
    luchanghong_3     26      male   shanghai
    ```

- 输出指定列（column）

    ```bash
    lch@localhost:Desktop $ awk '{print $1,$3}' student.txt
    name sex
    luchanghong male
    luchanghong_2 male
    luchanghong_3 male
    ```

- 输出指定行（row）

    ```bash
    lch@localhost:Desktop $ awk 'NR==2 {print}' student.txt
    luchanghong     24      male    beijing
    ```

- 查找

    ```bash
    lch@localhost:Desktop $ awk '/luchanghong/' student.txt
    luchanghong     24      male    beijing
    luchanghong_2     25      male   tianjing
    luchanghong_3     26      male   shanghai
    lch@localhost:Desktop $ awk '/luchanghong_/' student.txt
    luchanghong_2     25      male   tianjing
    luchanghong_3     26      male   shanghai
    # 按某一个field查找
    lch@localhost:Desktop $ awk '$4 ~ /g/' student.txt
    luchanghong     24      male    beijing
    luchanghong_2     25      male   tianjing
    luchanghong_3     26      male   shanghai
    lch@localhost:Desktop $ awk '$4 ~ /g$/' student.txt
    luchanghong     24      male    beijing
    luchanghong_2     25      male   tianjing
    ```

## 高级用法

这里的高级用法是相对上面初级而言的，高手尽管鄙视吧。

- 求和、均值、最值

    ```bash
    lch@localhost:Desktop $ awk '{totalAge+=$2} END {print totalAge}' student.txt
    75
    lch@localhost:Desktop $ awk 'NR>1 {num+=1;totalAge+=$2} END {print totalAge/num}' student.txt
    25
    lch@localhost:Desktop $ awk 'BEGIN {max=0} {if (int($2)>max) max=$2 fi} END {print max}' student.txt
    26
    ```

- 结合grep、sort、uniq等命令使用

    ```bash
    # 稍简单
    grep -n "Update [0-9]" updateShopItems.log | awk '{print $14}' | sort -n
    # 较复杂
    for x in $(grep startup /bypush/logs/nginx/access.log | grep -i zte | awk '{print $1}'); do python qqwry.py ${x}; done | grep 广东
    ```

## awk脚本

如果要执行的过程比较复杂，可以单独写一个awk脚本，然后调用。例如：

```bash
lch@localhost:Desktop $ vim foo.awk
1 #!/usr/bin/awk -f
2
3 BEGIN { print "Hello, awk." }
4 {
5     print $1, $2
6 }
7 END { print "Goodbye, awk." }
```

运行：

```bash
lch@localhost:Desktop $ awk -f foo.awk student.txt
Hello, awk
name age
luchanghong 24
luchanghong_2 25
luchanghong_3 26
Goodbye, awk
```

## 说明

awk默认是以`空格`来分隔每一行的字符串，分隔出来的字符串从1、2、3……开始计数，对应输出就是$1、$2、$3，如果数据不是按照空格来分隔的，也可以自定义分隔符，例如：

```bash
lch@localhost:Desktop $ echo "name:luchanghong" | awk -F ":" '{print $1,$2}'
name luchanghong
```
