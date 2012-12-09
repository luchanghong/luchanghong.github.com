---
layout: post
title: CodeIgniter中引用静态文件的路径问题
category: php
tags: [php,codeigniter]
description: 用CI框架做开发的时候，在模板中引用JS、CSS、Image等文件是必不可少的，在使用相对路径引用的时候有几个步骤，特总结一下。
---
## 定义base_url常量

在`application/config/config.php`里定义：

<pre class="prettyprint">
$config['base_url'] = "http://www.xxx.com";
</pre>

## 把url加载到autoload中

在`application/config/autoload.php`里定义：

<pre class="prettyprint">
$autoload['helper'] = array('url');
</pre>

## 在模板里定义base

在模板里（views目录下）加上一个HTML标签：

    <base href="<?php echo base_url();?>" />

然后，在模板里引用JS、CSS和图片的路径就可以写上相对于base_url的路径了。
