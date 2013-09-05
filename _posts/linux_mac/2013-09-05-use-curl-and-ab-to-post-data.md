---
layout: post
title: 使用Curl和ab post数据
catrgory: linux_OSX
tags: [curl, ab]
description: 之前一个月的时间都在忙碌公司的新产品——KK购物（目前只有iOS版），所以很久没有更新博客了，最近闲下来就把开发过程中遇到的问题总结归纳。今天说的curl，我用来测试接口；而ab自然是压力测试了。
---

## use curl to post data

为了测试接口，写个简单的脚本，截取一小段：

```bash
url="http://api.xxx.com/xxx"
d="uid=91&sid=3&act=yes"
result=$(curl -X POST --data "$d" ${url})
echo $result
```

## use ab to post data

做压力测试的时候用的：

```bash
ab -n 1000 -c 100 -p args.txt -T 'application/x-www-form-urlencoded' http://api.xxx.com/xxx > test_100.txt
```

要post的数据保存在一个文件里，如：

```bash
lch@localhost:Desktop $ vim args.txt
  1 uid=114&base=new&page=1
```

注意设置`-T`参数为`application/x-www-form-urlencoded`，默认则是`text/plain`。
