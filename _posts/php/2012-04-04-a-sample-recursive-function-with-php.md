--- 
wordpress_id: 72
wordpress_url: http://luchanghong.com/rosemary/?p=72
date: 2012-04-04 23:41:37 +08:00
layout: post
title: php递归函数举例
category: php
tags: [php, 递归]
description: 递归有时候觉得深不可测，其实很简单，就是循环调用，终止条件一般在函数内部。
---
突然想起来前些日子有人问我一个问题，我说写一个递归函数就可以解决当时的问题，但是他不知道怎么写，觉得递归循环很难的样子。

今天就举例，其实递归不难，要清楚要写的递归函数的参数形式要统一，而且要有终止的条件。下面例子：取出一个N维数组的所有值，返回一个一维数组。

```php
function getAllItem($item){
    global $item_arr;
    if (is_array($item)){
        foreach($item as $key=&gt;$val){
            getAllItem($val);       
        }
    }else{
        $item_arr[] = $item;
    }
    return $item_arr;
}
```

上面的例子很简单，逻辑不同复杂程度也就不同了，不过整体的思路是这样的。

PS：注意一定要有终止条件，否则死循环了！
