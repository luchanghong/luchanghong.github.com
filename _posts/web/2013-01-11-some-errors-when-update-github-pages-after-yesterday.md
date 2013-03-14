---
layout: post
title: github page build failure 的解决办法
description: 昨天更新一篇文章到github上，顺便把新浪微博的赞功能引用到博客中去，但是push本地的repository之后却收到`Page build failure`的邮件，还以为是加入新浪“赞”引起的，纠结了半天原来另有隐情！
tags: [github]
category: web
---

估计好多人也遇到这个问题吧，原因是github的jekyll更新版本了，在生成静态HTML文件的时候加强了语法和错误的检查（PS：也是处于安全考虑吧），所以过去可以忽略的一点小错误成了现在致命的伤。解决办法很简单，请往下看。

## 更新本地的jekyll

之前我的jekyll版本是`0.11.2`，现在是`0.12.0`，可以通过一下命令查看版本：

```bash
lch@localhost:~ $ jekyll -v
Jekyll 0.12.0
```

更新的话很简单，为了保险起见，我先卸载然后再安装：

```bash
sudo gem uninstall jekyll
sudo gem install jekyll
```

## 排查错误

安装新版本的jekyll之后就在本地排查错误吧：

```bash
jekyll --safe
```

会提示你的错误，我遇到的错误有两个：

1.  第一个是手误，把分类`tags: [svn]`写成了`category: [svn]`：

    ```ini
    Configuration from /Users/lch/dev/luchanghong.github.com/_config.yml
    Building site: /Users/lch/dev/luchanghong.github.com -> /Users/lch/dev/luchanghong.github.com/_site
    Liquid Exception: private method `gsub' called for ["svn"]:Array in post
    /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/lib/ruby/1.8/uri/common.rb:289:in `escape'
    ```

2.  第二个是之前引用的格式化日期的函数现在不能用了，具体原因我也不太清楚，只好把它干掉：

    ```ini
    Building site: /Users/lch/dev/luchanghong.github.com -> /Users/lch/dev/luchanghong.github.com/_site
    Liquid Exception: undefined method `xmlschema' for "Thu Jan 13 00:00:00 +0800 2011":String in post
    /Library/Ruby/Gems/1.8/gems/jekyll-0.12.0/bin/../lib/jekyll/filters.rb:57:in `date_to_xmlschema'
    ```

**注意：** jekyll的错误提示让人很纠结，因为不去报告具体位置，而且ruby这个我也没学过，只能靠自己的感觉了……

## 最后一步

这一步貌似很多余！排查完错误之后，能在本地运行`jekyll --server`跑起来就OK了，然后再把本地的repository push到github上去即可。

对你有用的话赶紧在下面赞一下吧，O(∩_∩)O哈哈~
