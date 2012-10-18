---
layout: post
title: 从python中copy与deepcopy的区别看python引用
category: python
tags: [python, copy]
description: 昨天遇到一个关于copy与deepcopy的问题，平时还真没注意，因为做WEB开发用到这两个是鲜有的，我曾经用到过一次，貌似也忘记了。不过字面上来看一个是正常的copy，一个是深copy。
---

讨论copy与deepcopy的区别这个问题要先搞清楚python中的引用、python的内存管理。

python中的一切事物皆为对象，并且规定参数的传递都是对象的引用。可能这样说听起来比较难懂，对比一下PHP中的赋值和引用就有大致的概念了。参考下面一段引用：
    1. python不允许程序员选择采用传值还是传引用。Python参数传递采用的肯定是“传对象引用”的方式。实际上，这种方式相当于传值和传引用的一种综合。如果函数收到的是一个可变对象（比如字典或者列表）的引用，就能修改对象的原始值——相当于通过“传引用”来传递对象。如果函数收到的是一个不可变对象（比如数字、字符或者元组）的引用，就不能直接修改原始对象——相当于通过“传值”来传递对象。
    2. 当人们复制列表或字典时，就复制了对象列表的引用同，如果改变引用的值，则修改了原始的参数。
    3. 为了简化内存管理，Python通过引用计数机制实现自动垃圾回收功能，Python中的每个对象都有一个引用计数，用来计数该对象在不同场所分别被引用了多少次。每当引用一次Python对象，相应的引用计数就增1，每当消毁一次Python对象，则相应的引用就减1，只有当引用计数为零时，才真正从内存中删除Python对象。

所谓“传值”也就是赋值的意思了。那么python参数传递有什么特殊呢？看例子：
<pre class="prettyprint">
>>> seq = [1, 2, 3]
>>> seq_2 = seq
>>> seq_2.append(4)
>>> print seq, seq_2
[1, 2, 3, 4] [1, 2, 3, 4]
>>> seq.append(5)
>>> print seq, seq_2
[1, 2, 3, 4, 5] [1, 2, 3, 4, 5]
</pre>

如果按照PHP的语法，seq和seq_2这两个变量对应两个不同的存储地址，自然对应不同的值，是毫无关联的，但是在python中确令我们大跌眼镜。再看下面的例子：
<pre class="prettyprint">
>>> a = 1
>>> b = a
>>> b = 2
>>> print a, b
1 2
>>> c = (1, 2)
>>> d = c
>>> d = (1, 2, 3)
>>> print c, d
(1, 2) (1, 2, 3)
</pre>

显然和上面的例子有冲突吗？看开头引用的话就明白了，当引用的原始对象改变的时候，他俩就没有关系了，也就是说他俩是两个不同对象的引用，对应各自引用计数加减1；而第一个例子中seq和seq_2都是对原始对象[1, 2, 3]这个lis对象的引用，所以不管append()还是pop()都不会改变原始对象，只是改变了它的元素，这样也就不难理解第二个例子了，因为b = 2就是创建了一个新的 int 对象。

接下来再通过例子看copy与deepcopy的区别：
<pre class="prettyprint">
>>> seq = [1, 2, 3]
>>> seq_1 = seq
>>> seq_2 = copy.copy(seq)
>>> seq_3 = copy.deepcopy(seq)
>>> seq.append(4)
>>> print seq, seq_1, seq_2, seq_3
[1, 2, 3, 4] [1, 2, 3, 4] [1, 2, 3] [1, 2, 3]
>>> seq_2.append(5)
>>> print seq, seq_1, seq_2, seq_3
[1, 2, 3, 4] [1, 2, 3, 4] [1, 2, 3, 5] [1, 2, 3]
>>> seq_3.append(6)
>>> print seq, seq_1, seq_2, seq_3
[1, 2, 3, 4] [1, 2, 3, 4] [1, 2, 3, 5] [1, 2, 3, 6]
</pre>

这个例子看不出copy之后和之前的联系，也看不出copy与deepcopy的区别。那么再看：

<pre class="prettyprint">
>>> m = [1, ['a'], 2]
>>> m_1 = m
>>> m_2 = copy.copy(m)
>>> m_3 = copy.deepcopy(m)
>>> m[1].append('b')
>>> print m, m_1, m_2, m_3
[1, ['a', 'b'], 2] [1, ['a', 'b'], 2] [1, ['a', 'b'], 2] [1, ['a'], 2]
>>> m_2[1].append('c')
>>> print m, m_1, m_2, m_3
[1, ['a', 'b', 'c'], 2] [1, ['a', 'b', 'c'], 2] [1, ['a', 'b', 'c'], 2] [1, ['a'], 2]
>>> m_3[1].append('d')
>>> print m, m_1, m_2, m_3
[1, ['a', 'b', 'c'], 2] [1, ['a', 'b', 'c'], 2] [1, ['a', 'b', 'c'], 2] [1, ['a', 'd'], 2]
</pre>

从这就看出来区别了，copy拷贝一个对象，但是对象的属性还是引用原来的，deepcopy拷贝一个对象，把对象里面的属性也做了拷贝，deepcopy之后完全是另一个对象了。再看一个例子：
<pre class="prettyprint">
>>> m = [1, [2, 2], [3, 3]]
>>> n = copy.copy(m)
>>> n[1].append(2)
>>> print m, n
[1, [2, 2, 2], [3, 3]] [1, [2, 2, 2], [3, 3]]
>>> n[1] = 0
>>> print m, n
[1, [2, 2, 2], [3, 3]] [1, 0, [3, 3]]
>>> n[2].append(3)
>>> print m, n
[1, [2, 2, 2], [3, 3, 3]] [1, 0, [3, 3, 3]]
>>> m[1].pop()
2
>>> print m, n
[1, [2, 2], [3, 3, 3]] [1, 0, [3, 3, 3]]
>>> m[2].pop()
3
>>> print m, n
[1, [2, 2], [3, 3]] [1, 0, [3, 3]]
</pre>

最后测试你到底掌握没有：
<pre class"prettyprint">
l = []
d = {'num': 0, 'sqrt': 0}
for x in [1, 2, 3]: 
    d['num'] = x 
    d['sqrt'] = x*x 
    l.append(d)
print l
</pre>

由于我主要从事WEB开发，平时主要用framework里面一下package，很少研究python自身的一些东西，不过话说回来，我是用python来工作的，也不是专门研究这门语言的教授之类。时间越久发现python越有趣。

参考资料：
[python的内存管理](http://www.cnblogs.com/codebean/archive/2011/05/27/2059879.html)