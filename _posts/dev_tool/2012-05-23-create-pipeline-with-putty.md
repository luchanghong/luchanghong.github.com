--- 
wordpress_id: 261
wordpress_url: http://luchanghong.com/rosemary/?p=261
date: 2012-05-23 17:04:20 +08:00
layout: post
title: !binary |
  cHV0dHnlu7rnq4vpmqfpgZM=
category: dev_tool
tags: [putty, 隧道]
description: 如果你的目标主机不在公网，或者不对你所在的网段开放，那么如何去访问它呢？这就要借助隧道技术了，学习一下 putty 的用法。 
---
由于服务器搬迁，所以不能再用内网去访问数据库了，因此用到了Putty。其实说“隧道”可能一下子不大明白，更通俗的讲应该说是通信的媒介，比如飞鸽传书中的鸽子~~Putty把放鸽子的人、鸽子、收鸽子的人联系起来也就是我们说的“隧道”了。

放鸽子（客户端）：127.0.0.1

鸽子（隧道媒介）：111.111.111.111

收鸽子（服务器）：222.222.222.222

目前情况肯定是客户端联系不上服务器，所以才会借助隧道（个人认为一般这样的服务器都不会接入公网，比如存放数据库的服务器，如果接入公网就没必要建立隧道了）。

一、下载Putty

<a href="http://www.putty.org/">http://www.putty.org/</a>

二、配置Putty
<ol>
	<li>配置信鸽：session -&gt; HostName
<a href="/upload/2012/05/putty1.jpg"><img class="alignnone size-full wp-image-262" title="putty1" src="/upload/2012/05/putty1.jpg" alt="" width="460" height="436" /></a>
写上Saved Session（起个名字），然后Save</li>
	<li>Connection -&gt; SSH -&gt; Tunnels
<a href="/upload/2012/05/putty21.jpg"><img class="alignnone size-full wp-image-264" title="putty2" src="/upload/2012/05/putty21.jpg" alt="" width="460" height="436" />
</a>然后点击“Add”</li>
	<li>保存配置
这里要注意，回到Session选项，然后save。可以再去 Connection -&gt; SSH -&gt; Tunnels看看有没有保存成功，如果OK，就Open即可。这样访问本地127.0.0.1:9000获取到的就是222.222.222.222:8000的信息了。</li>
</ol>
还有反向隧道等说法可以上网搜一下，注意的是关闭Putty之后再次打开要Load一下才能加载上之前的配置，然后再Open。

&nbsp;

&nbsp;
