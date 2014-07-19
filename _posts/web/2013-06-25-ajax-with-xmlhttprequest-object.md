---
layout: post
category: web
tags: [ajax]
title: 使用XMLHttpRequest实现AJAX
description: 一直使用jQuery内部封装好的$.ajax等函数，时间久了会形成依赖，甚至会忘记AJAX到底是怎么回事了，今天来重温XMLHttpRequest实现原生的AJAX的过程。
---

## AJAX

对于WEB开发者来说，`AJAX`这个词一点也不陌生，现在说说大概会想到以下几点：

- 无刷新改变页面数据

- 异步通信

- $.ajax、$.get、$.post

- Asynchronous Javascript And XML

- ...

`AJAX`就是一种交互式的网页应用技术，主要目的的提升用户体验，其原理是利用
`Javascript`的`XMLHttpRequest`对象与服务器通信获取数据后填充在页面中，从而实现
不刷新整个页面却能把信息反馈给用户的效果。

## 通常使用的AJAX

我们通常使用的`AJAX`应该绝大多数都是`jQuery`封装好的，所以时间久了我们会对`jQuery`
形成很大的依赖。而且，在有些使用场景里，我们只用到了`AJAX`却把整个`jQuery`文件包含
进来，这样会影像网页的加载时间（在用户网络不好的情况下更糟糕）。

所以，掌握原生的`AJAX`写法还是很有必要的，不仅能摆脱`jQuery`的依赖，还能进一步加深
对异步通信的理解。如果抽时间再去研究`XMLHttpRequest`对象那就更赞了。

## 原生的AJAX

- 生成XMLHttpRequest对象

    ```javascript
    var XMLReq;
    if (window.XMLHttpRequest) {
        XMLReq = new XMLHttpRequest();
    } else {
        XMLReq = new ActivateXObject('Microsoft.XMLHTTP');
    }
    ```

    在`IE`浏览器里没有`XMLHttpRequest`这个对象，要使用
    `ActivateXObject('Microsoft.XMLHTTP')`来替代。

- 向服务器发送请求

    ```javascript
    // for GET method
    XMLReq.open('GET', 'ajax_get.html', true);
    XMLReq.send();

    // for POST method
    XMLReq.open('POST', 'ajax_post.html', true);
    XMLReq.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    XMLReq.send('key=value&key2=value2');
    ```

    上面代码中`XMLReq.open()`的第三个参数就是控制是否异步请求的，当为true的时候
    就是我们说的异步通信，为false的时候就按照正常的javascript逻辑顺序执行。真正
    异步的目的就是在向服务器请求数据的时候不影响正常的javascript代码执行，否则
    一旦请求时间过长或者出错可能影响整个页面的浏览。

- 处理服务器响应数据

    ```javascript
    XMLReq.onreadystatechange = function () {
        if (4 == XMLReq.readyState && 200 == XMLReq.status) {
            var data = XMLReq.responseText;
            // return xml data
            // var data = XMLReq.responseXML;
        }
    }
    ```

    一个完整的`AJAX`请求过程可以对照`XMLReq.readyState`参数清晰的理解清楚，不同的
    值表示过程如下：

    | readyState |  过程变化 |
    | ---------- | --------- |
    |   0        | 请求未初始化 |
    |   1        | 服务器连接已建立 |
    |   2        | 请求已接收 |
    |   3        | 请求处理中 |
    |   4        | 请求已完成，且响应已就绪|

## Simple Demo

ajax.html

```html
<html>
    <script type="text/javascript" charset="utf8">
        function my_ajax(url, callback) {
            var XMLReq;
            if (window.XMLHttpRequest) {
                XMLReq = new XMLHttpRequest();
            } else {
                XMLReq = new ActivateXObject('Microsoft.XMLHTTP');
            }

            XMLReq.onreadystatechange = function() {
                // alert(XMLReq.readyState);
                if (4 == XMLReq.readyState && 200 == XMLReq.status) {
                    callback(XMLReq.responseText);
                }
            }

            XMLReq.open('GET', url, false);
            XMLReq.send();
        }

        function btn_click() {
            my_ajax('ajax_get.html', function (data) {
                    document.getElementById('result').innerHTML = data;
                }
            );

            alert('ok');
        }
    </script>

    <body>
        <input type="button" value="Click" onclick="btn_click()" />
        <div id="result"></div>
    </body>
</html>
```

ajax_get.html

```html
<div> I am ajax get data </div>
```

本文意在抛砖引玉，有空的时候能看一看原生的东西，尽管比较复杂。
