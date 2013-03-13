---
layout: post
title: 使用Redcarpet和Highlight.js
tags: [a,b]
description: 今天把prettyprint替换掉，采用了Highlight.js来给代码上色。因为在使用prettyprint的时候比较麻烦，体现不出MarkDown的优势，为了迎合Hightlight.js使用更加简介的代码块编辑风格，但是原有的MarkDown解释器能力有限，于是采用Redcarpet作为新的解释器。
---

## 使用Highlight.js

- 下载Highlight

[Highlight][1]是一款专门为MarkDown打造的，支持54种编程语言的代码高亮和26种代码风格。进入[下载页面][2]选择你使用的语言，然后点击`download`按钮下载，完成之后解压，把`highlight.pack.js`和`style`目录里你喜欢的代码风格样式文件拷贝到项目中去。

[1]: http://softwaremaniacs.org/soft/highlight/en/ "Highlight"
[2]: http://softwaremaniacs.org/soft/highlight/en/download/ "Highlight download"

- 使用Highlight

通常在`layout`页面引用一次即可，例如：

```html
<link rel="stylesheet" href="/css/tomorrow-night.css" type="text/css" media="screen, projection" />
<script src="/js/highlight.pack.js" type="text/javascript"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

**注意引用的路径**

- 代码块写法

指定代码的语言，形如：

<pre class="no-highlight">
```php
    $a = 'a';
    echo $a;
```
</pre>

但是之前使用的`rdiscount`这个解释器对上面代码解析错误，所以要使用下面的`redcarpet`。

## 使用Redcarpet

- 安装redcarpet

```bash
lch@localhost:luchanghong.github.com $ sudo gem install redcarpet
Password:
Fetching: redcarpet-2.2.2.gem (100%)
Building native extensions.  This could take a while...
Successfully installed redcarpet-2.2.2
1 gem installed
Installing ri documentation for redcarpet-2.2.2...
Installing RDoc documentation for redcarpet-2.2.2...
```

- 修改_config.yml

```
markdown: redcarpet
redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "with_toc_data"]
```
