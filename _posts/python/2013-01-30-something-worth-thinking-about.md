---
layout: post
title: 思考，在写了一个python脚本后
tags: [python]
category: python
description: 最近为后台管理员写了一个批量上传的脚本，就是把信息录入到EXCEL表格中去，把对应的图片等资料按照规定的格式打包上传到服务器，然后执行我这个脚本就OK了。我想说的不是我写的这个脚本有多么复杂、用了哪些package，而是分享一下开发过程中我遇到并认为值得思考的东西。
---

## 时间不仅仅是把杀猪刀

时间的威力之大，我们每个人都不得不屈服于TA。因为最近做PHP开发，python又成了一个熟悉的陌生人，大概4个月没有写python了，只是偶尔看看群里面讨论的东西。昨天在写的时候觉得好多东西就在手边，但是就差一点想不起来，不过点滴的回忆也能促使旧情复燃，写的过程中倒也蛮愉快，毕竟当初对python用情很深！

**思考** `曾经用情至深，时间磨灭不掉`

## 解决难题后快感倍增

python比较头疼是装一些package的时候可能会出现这样那样的问题，对系统和环境的依赖性较强。点背的时候怎么也安装不了或者import不进来，不过我还是相信我今天遇到的问题很可能是某人N年以前纠结的地方，于是google一番之后终于找到答案，瞬间快感袭来，战斗力增加100%。

**思考** `解决难题的两种方式：要么前车之鉴，要么开拓创新`

## 水不一定要装在杯子里

这个小标题是我突然想到的，意思就是换一种方式去处理事情。总所周知，“汉字”这个中国特有的东西给我们coder带来了多少麻烦，特别是在读取存储转换的时候。开发过程中遇到一个问题：从文件中读取信息，然后用函数处理，最后保存到数据库中。处理的过程uft8好unicode转来转去，最后还出错了……地铁里灵光一现：把含有汉字的字符串用某种方法转换成一种普通字符（比如我用urlencode的思路），然后再释放出来（urldecode）。突然觉得像是加密一样，重要的是可逆。

**思考** `把麦子放在自己背上，其实主人和毛驴都很开心`

## 错误更重要

程序是为了给我们预期的结果，如果达不到预期肯定是处理过程中出错了（在保证输入的前提下），所以错误很重要，更重要的捕捉错误。发现我的程序里好多try……except，有错误只是简单的print做调试使用，做得好一点就写个方法把错误记录保存成对应的log日志。

**思考** `我绝对不会在同一个地方跌倒两次`

## 花心=好学？

男人喜欢好几个女人，他花心；我喜欢好几门语言，我花心吗？貌似没有什么可比性，所以我前一段时间在学Objective-C。

**思考** `你深入学习一门语言的时候自然就涉猎其他的语言，不是厌倦，是好奇`