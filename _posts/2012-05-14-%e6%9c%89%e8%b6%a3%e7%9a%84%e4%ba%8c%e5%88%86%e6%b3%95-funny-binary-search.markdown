--- 
wordpress_id: 186
wordpress_url: http://luchanghong.com/rosemary/?p=186
date: 2012-05-14 16:07:47 +08:00
layout: post
title: !binary |
  5pyJ6Laj55qE5LqM5YiG5rOV4oCU4oCUZnVubnkgYmluYXJ5IHNlYXJjaA==

---
记得上大学的时候有门课程叫什么算法的，讲到了二分法，二分法不言言而喻，就是一半一半的查找。当时的公式早就不记得了，下面用Python实现一下，也蛮简单的。
<pre class="prettyprint">
#-*- coding: utf-8 -*-
__author__ = 'luchanghong'

def binarySearch(list, v):
    low = 0
    high = len(list) - 1
    num = 0

    while low &lt;= high:
        num += 1
        average = (low + high) // 2
        cur_val = list[average]
        #print cur_val
        if cur_val &gt; v:
            high = average - 1
        elif cur_val &lt; v:
            low = average + 1
        elif cur_val == v:
            return cur_val, num
    return 'no found, %d' % num

if __name__ == '__main__':
    list = range(100)
    #print binarySearch(list, 98)
    res = []
    for x in list:
        res.append(binarySearch(list, x))
    res.sort(key = lambda l:l[1], reverse = True)
    print res
</pre>
也许，会怀疑这样的效率，比如不同查找按大小顺序匹配，可能0用1次就查找到了，但是99就要100次才能查找，到所以我就把所有的数字查找次数放在一块比较了一下，发现最多查找也只有7次，最少的一次是49，注意我这里取的数组是0~99，综合一下还是二分法查找效率高~~~

<a href="/upload/2012/05/binary.jpg"><img class="alignnone size-full wp-image-191" title="binary" src="/upload/2012/05/binary.jpg" alt="" width="681" height="258" /></a>
