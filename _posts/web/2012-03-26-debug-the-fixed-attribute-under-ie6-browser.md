--- 
wordpress_id: 40
wordpress_url: http://luchanghong.com/rosemary/?p=40
date: 2012-03-26 17:29:29 +08:00
layout: post
title: !binary |
  6Kej5YazSUU25LiN5pSv5oyBZml4ZWTlsZ7mgKfnmoTpl67popg=
category: UI
tags: [css]
description: 做前端开发不是我的强项，虽然也能简单搞个博客，但是最头疼的关于兼容性的问题始终存在。今天来看一个 IE6 下关于 fixed 属性的问题。
---
IE6不支持fixed属性，记录一下解决方法。

想让我的一个DIV块浮动在右上角的地方，不随滚屏而变化。

<pre class="prettyprint">

&lt;div id="portamento_container" style="width: 200px;"&gt;This is my text!&lt;/div&gt;

</pre>

下面是css样式写法

[css]

/* 除IE6浏览器的通用方法 */
#portamento_container{position:fixed;right:0;top:0;background:green;width:200px;height:20px;}

/* IE6浏览器的特有方法 */
* html #portamento_container{position:absolute;right:expression(eval(document.documentElement.scrollLeft+10));top:expression(eval(document.documentElement.scrollTop+10))}

/*防止抖动*/
* html,* html body{background-image:url(about:blank);background-attachment:fixed}

[/css]

如果要改变DIV浮动的位置，就改变CSS里面的right、left、top、bottom等属性即可
