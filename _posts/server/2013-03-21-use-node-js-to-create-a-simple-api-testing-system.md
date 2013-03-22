---
layout: post
category: server
tags: [node.js]
title: 使用Node.js搭建一个简单的API测试系统
description: 项目需要做一套API接口，但是开发需要时间，就先模拟一套API测试接口。接口中数据传输格式都是JSON，为了方便就选用Node.js来实现，每个接口都直接输出JSON字符串即可。
---

## Node.js

> Node.js is a platform built on [Chrome's JavaScript runtime][1] for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

关于Node.js的介绍就不必多说了，请参考下面的资料：

-   [Node.js 官网][2]
-   [Node.js 维基百科][3]

[1]: http://code.google.com/p/v8/
[2]: http://nodejs.org/ "Node.js 官网"
[3]: http://zh.wikipedia.org/wiki/Node.js "Node.js 维基百科"

## Node.js 入门

我是从一个网站开始学习Node.js的，当然只能算是入门，你也可以跟着官网的文档学习。关于Node.js的webframework，可以看看国外的`expressjs`和国内的`rrestjs`，不管什么语言，大多都逃离不掉MVC的设计思路。

-   [Node.js 入门][4]
-   [expressjs 官网][5]
-   [rrestjs 官网][6]

[4]: http://www.nodebeginner.org/index-zh-cn.html "Node.js 入门"
[5]: http://expressjs.com/ "expressjs 官网"
[6]: http://rrest.cnodejs.net/about "rrestjs 官网"

## API测试DEMO

我只是随便看一下最简单的教程，写的不是那么规范吧，不过是原生的哈！[DEMO点此下载][DEMO]，只提供新新手使用，下载DEMO后解压，运行：

```bash
lch@localhost:node.js.demo $ node index.js
It has started.
```

[DEMO]: /upload/attachement/20130322/node.js.demo.tar.gz
