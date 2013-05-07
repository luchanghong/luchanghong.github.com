--- 
wordpress_id: 21
wordpress_url: http://luchanghong.com/rosemary/?p=21
date: 2012-03-21 22:40:12 +08:00
layout: post
title: jquery配置ckeditor
category: UI
tags: [ckeditor, jquery]
description: 富文本编辑器在 WEB 网站中四很常见的，比如 FCKeditor 、 CKEditor。来看一下 jQuery 快速配置使用 CKEditor 。
---
做WEB开发的过程中，特别是后台的开发，搭建一个新闻发布CMS是经常要做的，当然会用到富文本编辑器。

ckeditor是常用的，无论你用什么语言开发，都要做一番配置才行。之前做PHP类的后台，配置也稍有点麻烦，前几天在做一个后天的CMS的时候，觉得挺别扭，难道我用python开发的也要配？下载一个新版本的ckeditor，看一下_samples，下面有个<span style="color: #ff0000;">CKEditor Sample — Using jQuery Adapter，看一下很简单配置一下：</span>

## 1.导入所需要的js

```javascript
<script type="text/javascript" src="../ckeditor.js"></script>
<script type="text/javascript" src="../adapters/jquery.js"></script>
```

## 2.页面定义一个texarea文本输入框，class指定一个值，例如

```
class="textarea_class"
```

3.加入下面的一段js代码

```javascript
$(function()
{
    var config = {
        skin:'v2'
    };

    $('.textarea_class').ckeditor(config);
});
```
