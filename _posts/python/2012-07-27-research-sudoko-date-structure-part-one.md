--- 
wordpress_id: 456
wordpress_url: http://luchanghong.com/rosemary/?p=456
date: 2012-07-25 16:08:40 +08:00
layout: post
title: !binary |
  5o6i56m25pWw54us566X5rOV77yI5LiA77yJ
category: python
tags: [python, 数独]
description: 昨天在地铁看见有人拿着 ipad 玩数独游戏，想必好多人都玩过。不过我没有想着怎么尽快的把数字填出来，而是想着怎样生成一组能够算的上是“数独”的数据，如果你也感兴趣就跟我一探究竟吧。
---
昨天在地铁看见有人拿着pad玩数独游戏，说“数独”估计好多人都蒙了，看下游戏截图就明白了，记得上初中的时候在电子词典上玩过。

<a href="/upload/2012/07/sudoko.jpg"><img class="alignnone size-full wp-image-457" title="sudoko" src="/upload/2012/07/sudoko.jpg" alt="" width="410" height="410" /></a>

&nbsp;

言归正传，游戏的核心就是实现9*9的格子，而且要横、竖都有1—9这9个数字，也就是不能重复出现。

一、实现两行数字

先随机一个数组，然后再生成一个数组，组成两行。第一个随机数组很简单，那么如何构造第二个数组呢？我的思路：第二个数组的第一个元素肯定不能和第一个数组的第一个元素相同，也就是list2[0]!=list1[0]，那么第二个元素肯定也不等于list1[1]，并且不能和之前出现过的第一个元素相同，依次类推：第二行的第N个元素不等于第一行第N个元素，也不等于第二行的前N-1个元素。首先有个固定不变的初始数组range(1, 10)，那么每一次构造元素的时候，就先把不能出现的元素组成一个数组，然后和初始数组做差集，那么将要构造的元素肯定就是差集里面随机的一个元素了。
<pre class="prettyprint">
# _*_ coding:utf8 _*_
import random

def get_rand_item(l):
    random.shuffle(l)
    return l[0]

# init number list
num_list = range(1, 10)

# create row_1
row_1 = range(1, 10)
random.shuffle(row_1)
print row_1

# create a new row
new_row = list()
for j in range(0, 9):
    # 已经出现过的数字
    select_list = list(new_row)
    # 第一行同一个位置的数字 
    select_list.append(row_1[j])
    #print select_list

    # 和初始数组的差集
    remain_list = [x for x in num_list if x not in select_list]

    if remain_list:
        new_num = get_rand_item(remain_list)
        new_row.append(new_num)
        #print remain_list
        #print new_row
    else:
        new_row.append(row_1[j])

print new_row
</pre>
程序看上去很简单，但是第一次写的时候没有做if……else判断，有时候得到的数组只有8个数字，这是为什么呢？

第一次：已出现数字 new_row ([]) + 第一行第一个数字 row_1[0]  = select_list，此时select_list有一个元素

第二次：已出现数字new_row([x]) + 第一行第二个数字row_1[1] = select_list，此时select_list有两个元素

……

第八次：select_list有8个元素

第九次：select_list有9个元素，和初始数组num_list做差集要么是个空数组，要么是个只有一个元素的数组

空数组是特殊情况，出现原因：第二行前8个数字中不包含第一行最后一个数字row_1[8]，导致select_list是全部不重复的9个数字，这样差集就是[]，所以我暂且做个处理在这种情况下把第一行的最后一个数赋值给第二行的最后一个数。

OK，先到这了，北京最近多暴雨，今天公司提前下班，回家先。

&nbsp;
