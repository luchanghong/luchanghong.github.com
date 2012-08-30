--- 
wordpress_url: http://luchanghong.com/rosemary/?p=342
date: 2012-07-02 11:06:36 +08:00
layout: post
wordpress_id: 342
title: !binary |
  UHl0aG9u55qEYmNyeXB05Yqg5a+G5ZKMbWQ15Yqg5a+G

---
数据存储和传输总是担心不安全，于是乎有了各种加密算法，什么对称、非对称之类……用的最多的当然还是md5，虽然不可逆，但是暴力破解让你的明文现原形。我用Python除了上一篇文章说的<a title="python下RSA加密解密以及跨平台问题" href="http://luchanghong.com/rosemary/?p=333">RSA</a>加密之外，也用到了bcrypt和md5。

一、bcrypt

验证方式和之前用的不同，不是直接解密得到明文，也不是二次加密比较密文，而是把明文和存储的密文一块运算得到另一个密文，如果这两个密文相同则验证成功。
<pre>[python]
&gt;&gt;&gt; import bcrypt
&gt;&gt;&gt; s = 'hello'
&gt;&gt;&gt; hash = bcrypt.hashpw(s, bcrypt.gensalt())
&gt;&gt;&gt; print hash
$2a$12$1VwtpKmC77PkaoTol0HIS.Wqp24FUNHcB2OyPLPQBwVO.P3NVEwWq
&gt;&gt;&gt; hash2 = bcrypt.hashpw(s, hash)
&gt;&gt;&gt; hash == hash2
True
[/python]</pre>
每次加密密文都不同，安全性能大大提高了。

二、md5

这个验证方式就简单了，把明文加密之后和存储的密文比较是否一致。
<pre>[python]
&gt;&gt;&gt; from hashlib import md5
&gt;&gt;&gt; s = 'hello'
&gt;&gt;&gt; m = md5(s)
&gt;&gt;&gt; m.hexdigest()
'5d41402abc4b2a76b9719d911017c592'
[/python]</pre>
&nbsp;
