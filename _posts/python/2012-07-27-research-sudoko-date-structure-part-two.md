--- 
wordpress_id: 467
wordpress_url: http://luchanghong.com/rosemary/?p=467
date: 2012-07-26 16:19:45 +08:00
layout: post
title: 探究数独算法（二）
category: python
tags: [python, 数独]
description: 第一步已经实现了两行数字——两个数组，那么根据第二行的构造方法再来实现剩下的7行吧。为了看清楚上一篇文章说的那个特殊情况，就把那个数字用“X”代替。
---
今天突然意思到题目好像不太准确，我写的不是数独的解法，而是数独数据构造的方法，顺便也声明一下：还没有看别人是怎样算的，我只是凭兴趣自己想了一想，然后就来实现了，可能算法很简单粗暴，请见谅！那就开始第二步吧：

## 二、再构造7行数字

第一步已经实现了两行数字——两个数组，那么根据第二行的构造方法再来实现剩下的7行吧。为了看清楚上一篇文章说的那个特殊情况，就把那个数字用“X”代替，这个过程很简单，就是一个for循环。

```python
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

print '-'*30

sudoko_list = list()
sudoko_list.append(row_1)

for i in range(1,9):
    new_row = list()
    for j in range(0, 9):
        col_select = [x[j] for x in sudoko_list]
        row_select = list(new_row)
        select_list = col_select + row_select

        remain_list = [x for x in num_list if x not in select_list]
        if not remain_list:
            remain_list = ['X']
            #remain_list = [x for x in num_list if x not in new_row]
        new_num = get_rand_item(remain_list)
        new_row.append(new_num) 

    sudoko_list.append(new_row)

for l in sudoko_list:
    print l
```

方便对比，我把第一步代码也写在上面了，最终得到一个二维数组

```python
[4, 1, 5, 9, 8, 6, 2, 7, 3]
[1, 5, 6, 4, 3, 9, 8, 2, 7]
------------------------------
[4, 1, 5, 9, 8, 6, 2, 7, 3]
[6, 5, 9, 1, 4, 2, 8, 3, 7]
[5, 7, 3, 6, 9, 1, 4, 8, 2]
[9, 2, 1, 7, 5, 8, 6, 4, 'X']
[3, 6, 7, 5, 1, 4, 9, 2, 8]
[8, 3, 2, 4, 6, 5, 1, 9, 'X']
[1, 9, 8, 3, 7, 'X', 5, 6, 4]
[2, 4, 6, 8, 3, 7, 'X', 1, 9]
[7, 8, 4, 2, 'X', 9, 3, 5, 6]
```

这种特殊情况出现概率还蛮大的，那么接下来就要屏蔽掉这些特殊行。
