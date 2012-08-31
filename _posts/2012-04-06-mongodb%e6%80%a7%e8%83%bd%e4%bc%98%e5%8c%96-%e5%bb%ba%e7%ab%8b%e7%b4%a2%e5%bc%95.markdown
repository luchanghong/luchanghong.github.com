--- 
wordpress_id: 82
wordpress_url: http://luchanghong.com/rosemary/?p=82
date: 2012-04-06 16:11:51 +08:00
layout: post
title: !binary |
  bW9uZ29kYuaAp+iDveS8mOWMluKAlOKAlOW7uueri+e0ouW8lQ==

---
目前公司网站升级，数据库不再是mysql了，WEB端用到了mongodb。首先把数据从mysql导入到mongodb，总共1680多万记录，花了大概两个小时。

<a href="/upload/2012/04/KF45T7I8YTT5Q3C@C7.jpg"><img class="alignnone size-full wp-image-83" title="KF45T7I8Y](`T)T5Q3C@[C7" src="/upload/2012/04/KF45T7I8YTT5Q3C@C7.jpg" alt="" width="280" height="48" /></a>

接下来的工作是把这些数据按照这两个字段date和appKey归档处理，也就是分类求和。在mysql中常用SQL语句做这些工作，比如 sum(xxx)……group by date。在mongodb可以用db.collection.group()来做。

就拿date举例，直接在1680W的数据里做group()运算肯定是不可行的。那么我先做一个日期循环，按照当天的日期区间进行运算，直接就这样做，我记录下结果：

<a href="/upload/2012/04/1.jpg"><img class="alignnone size-full wp-image-85" title="1" src="/upload/2012/04/1.jpg" alt="" width="642" height="134" /></a>

上面两条处理时间间隔36s、18s、18s……我要处理一年多的数据，总共440天，这样需要时间：440 * 20 = 8800 s，也就是2.4小时，这个速度实在是慢。

我尝试做一下索引：<pre class="prettyprint">db.report_detail.ensureIndex({'date':-1})</pre>，按照时间逆序的索引。然后在执行数据处理，结果如下：

<a href="/upload/2012/04/IBPW3M8G_WNTG99OKU.jpg"><img class="alignnone size-full wp-image-87" title="`I(BPW]3M8G_WNTG99OK)`U" src="/upload/2012/04/IBPW3M8G_WNTG99OKU.jpg" alt="" width="641" height="254" /></a>

这次处理时间间隔约为2s，而且比较稳定，差距一下子就出来了。这样子算下来只需要15分钟就可以了。

然后在处理appKey相关数据，在没有添加相应的所以之前，处理效率如下：

<a href="/upload/2012/04/CENESB2KKKV_6ES08WR5PG.jpg"><img class="alignnone size-full wp-image-88" title="CENESB(2KKKV_6ES08WR5PG" src="/upload/2012/04/CENESB2KKKV_6ES08WR5PG.jpg" alt="" width="681" height="440" /></a>

处理时间间隔是7s左右，做appKey索引：<pre class="prettyprint">db.report_detail.ensureIndex({'appKey': 1})</pre>，再测试：
<div><a href="/upload/2012/04/RW8VWC4WCAA_7SRB7KJJ.jpg"><img class="alignnone size-full wp-image-89" title="RW8(VWC{`4WCAA_7SRB7KJJ" src="/upload/2012/04/RW8VWC4WCAA_7SRB7KJJ.jpg" alt="" width="681" height="440" /></a></div>
此时，处理时间间隔十几到几十毫秒，相差很大。最后的总记录是7831条。

这样，总的算下来，

节约时间：16*440 + 7831 * 6.5 = 57941.5 s  = 16 h

现在只需：2*440 + 7831 * 0.1 = 1663.1 s = 27 m

看来mongodb的优化提升空间是非常的大，我上面只是做单一的索引，有时候可以根据需要做组合索引，索引键值前后顺序不同，效率也不同。
