--- 
wordpress_id: 159
wordpress_url: http://luchanghong.com/rosemary/?p=159
date: 2012-05-07 17:46:14 +08:00
layout: post
title: 用pyQT开发的数字记忆游戏（一）
category: python
tags: [python, pyQT]
description: 打算用 pyQT 做一个数字记忆的小游戏，缘起好久之前看到一个朋友用 Android 做的一个类似的游戏。
---
之前看过一个朋友做的android手机游戏，考验记忆力的，很好玩，打算用pyQT复制一下，顺便练习练习。游戏玩法：棋盘格子一样的桌面，有几个连续整数随机出现在格子里，记住数字出现的位置，然后按照从小到大的顺序依次把数字找出来，很简单吧。

目前我把界面做出来了，随机出现的数字也有了，截图：

<a href="/upload/2012/05/memory.jpg"><img class="alignnone size-full wp-image-160" title="memory" src="/upload/2012/05/memory.jpg" alt="" width="420" height="436" /></a>

这样在数字被隐藏之后，由于每一个单元格都是一样的样式，所以这个难度太大，果断把没有数字的单元格背景去掉，如下图：

<a href="/upload/2012/05/memory2.jpg"><img class="alignnone size-full wp-image-161" title="memory2" src="/upload/2012/05/memory2.jpg" alt="" width="420" height="436" /></a>

第一次点击的时候游戏正式开始，当然会有个类似计数器的东西，第一次点击把所有数字隐藏，而且判断是不是1，依次累加即可做出相应的判断。当完成一次的时候，可以进入下一关，也就是多一个数字，挺有意思的益智小游戏吧。
