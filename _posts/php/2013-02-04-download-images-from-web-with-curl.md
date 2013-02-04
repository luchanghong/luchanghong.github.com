---
layout: post
category: php
tags: [php, curl]
title: 使用Curl下载网络图片
description: 在使用sina、tencent等开放API接口的时候，需要保存用户的头像，在具体实现的过程中对比了两种方法，最终选择CURL读取的方式。
---

## CURL方法

使用之前要开启CURL扩展，这个是必须的。`Tip：好像之前遇到过php 5.2.**的一个版本安装不了CURL扩展，这是个bug。`

<pre class="prettyprint">
function save_img_curl($avatar_url) {
    $res = curl_init($avatar_url);
    ob_start();
    curl_exec($res);
    $avatar_file = ob_get_contents();
    ob_end_clean();
    $info = curl_getinfo($res);
    curl_close($res);
    echo $info['total_time']."\n";
    
    // save avatar
    $up_dir = "./img/";
    if (!is_dir($up_dir))
    {   
        @mkdir($up_dir, 0777);
    }   
    $avatar_name = time().'.jpg';
    $handle = fopen($up_dir.$avatar_name, 'a');
    fwrite($handle, $avatar_file);
    fclose($handle);
    return;
}
</pre>

**注意：**我这里直接用`.jpg`作为扩展名了，实际上要根据`$info['content-type']`来决定。

## readfile方法

<pre class="prettyprint">
function save_img_curl($avatar_url) {
    $time_start = time();
    ob_start(); 
    readfile($avatar_url);
    $avatar_file = ob_get_contents();
    ob_end_clean();
    $time_end = time();
    echo "\n".$time_end-$time_start."\n";
}
</pre>

## 对比&疑惑

对比两种方法，选用了CURL方式，同时有一些疑问：

 * 对比两种方法，在去读sina的图片的时候都差不多，但是读取qq图片的时候差别很大，不知道为何？
 * 获取sina图片总是比qq要快很多，难道和网络有关？但是差距太大了

不知道有没有遇到我这种情况的，不妨探讨一下！
