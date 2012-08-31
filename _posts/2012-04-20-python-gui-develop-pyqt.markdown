--- 
wordpress_id: 134
wordpress_url: http://luchanghong.com/rosemary/?p=134
date: 2012-04-20 12:14:51 +08:00
layout: post
title: python GUI develop--pyQT
---
最近想学Python的GUI开发，在网上找找，在wxPython和pyQT之间徘徊。

昨天尝试了一下wxPython，配套开发软件选用的是wxGlade，感觉界面很古老的样子，像是80/90年代的计算机软件界面。

<a href="/upload/2012/04/wxglade.jpg"><img class="alignnone size-full wp-image-135" title="wxglade" src="/upload/2012/04/wxglade.jpg" alt="" width="734" height="363" /></a>

虽然是个GUI工具，主要的就是布局了，但是它可不支持拖动，操作手感不是很好。另外发现，虽然可以支持中文，但是保存之后再次打开编辑，有中文的组件会全部丢失，这点最悲剧了。

上午到公司后，还是果断听了同事的建议，还是学习QT吧：
<ol>
	<li>先去百度百科看一下QT介绍了</li>
	<li>然后去QT官网下载开发工具，根据操作系统选择，用python开发不需要下载整个SDK，下载他的Library就可以了
<a href="http://www.riverbankcomputing.co.uk/software/pyqt/download">http://www.riverbankcomputing.co.uk/software/pyqt/download</a></li>
	<li> 下载pyQT的扩展包，根据操作系统和python的版本选择下载
<a href="http://www.riverbankcomputing.co.uk/software/pyqt/download">http://www.riverbankcomputing.co.uk/software/pyqt/download</a></li>
</ol>
我用的分别是：PyQt-Py2.6-x86-gpl-4.9.1-1.exe和Qt libraries 4.8.1 for Windows (VS 2008, 234 MB)

下载的时候需要比较长的一段时间，再次期间，去浏览一下QT的官网，Nokia还是很给力的，网站有中文版，这样就比较容易浏览想要的资源了。再次对比pyQT和wxPython，虽然都开源了，但是QT有企业版，Nokia维护的也很给力，官方学习资料和doc也很全面，选择QT是明智的。

装好了QT之后，看一下Examples and Demos，马上就被吸引了。

<a href="/upload/2012/04/qt.jpg"><img class="alignnone size-full wp-image-136" title="qt" src="/upload/2012/04/qt.jpg" alt="" width="804" height="598" /></a>

QT大部分都是用C++开发的，但是QT跨平台和移植性都很强悍，用Python开发的应用都可以移植到其他平台上用。在移动互联网，QT程序可以移植到symbian手机上，现在Nokia又和Microsoft合作推出winphone系统，将来也有可能让QT支持winphone哦，Android和iOS也都说不定哦，前途还是很不错的嘛。
