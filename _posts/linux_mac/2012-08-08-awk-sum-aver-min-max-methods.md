--- 
wordpress_id: 487
wordpress_url: http://luchanghong.com/rosemary/?p=487
date: 2012-08-08 16:57:43 +08:00
layout: post
title: 【转】awk实现求和、平均、最大值和最小值的计算操作
category: linux_OSX
tags: [linux, mac, awk]
description: 在 shell 里面执行一些命令，如分析 log ，有时候会把符合的 log 输出行做一些简单的运算， akw 在此时就发挥出了效果。
---
1、求和

```
cat data|awk '{sum+=$1} END {print "Sum = ", sum}'
```

2、求平均

```
cat data|awk '{sum+=$1} END {print "Average = ", sum/NR}'
```

3、求最大值

```
cat data|awk 'BEGIN {max = 0} {if ($1&gt;max) max=$1 fi} END {print "Max=", max}'
```

4、求最小值（min的初始值设置一个超大数即可）

```
awk 'BEGIN {min = 1999999} {if ($1&lt;min) min=$1 fi} END {print "Min=", min}'
```

5、求访问次数的Top 10 Resource，可以根据此进行优化

```
cat output/logs/cookie_logs/`date +%u`/cookie_log|grep -v '172.16'|grep -v '127.0.0.1' |awk -F' '  '{ if(index($1,"219.141.246")!=0) print $2; else print $1  } '|sort|uniq -c|sort -n |tail -n 10
```

原文参考：http://www.2cto.com/os/201107/97785.html
