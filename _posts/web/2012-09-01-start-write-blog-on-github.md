---
layout: post
title: 开始在github上写博客了
category: web
tags: [github]
description: github 为我们提供了 pages 服务，而且能绑定我们自己的域名。github 的服务器很稳定，速度也不错，还省去了租用 WEB 服务器的钱，你值得拥有。本文分享了从 WordPress 转战到 github 的过程。
---

折腾一两天，终于把博客从 wordpress 转到 guthub 上来了，之前买的空间还有几个月就到期了，现在也不用考虑续费了，感谢 github 啊，每年省点 money 。
关于怎样在 github 上搭建一个网站，网上也有好多教程，不过都不是很适合像我这样之前重来都没有用过 github 的人，下面就来把我的过程描述一下。

## 申请一个 [github][] 账号

- 申请账号之后创建一个 `repository` ，我在 github 上的用户名是 luchanghong ，那么 repository 就命名为： `luchanghong.github.com`

- 往仓库加点东西。我们将要创建的网站都是基于 jekyll 的，看一下他在 github 上的主页，里面有介绍和用法，顺便也可以 clone 下来，里面的例子可以
直接copy到我们的仓库里。

PS：你也可以 clone 其他的 repository ，[这里][] 是一些使用 jekyll 的网站。

## 了解一下 jekyll ，并搭建本地 ruby 环境

可以去百度百科了解一下 [jekyll][] ，然后搭建本地 ruby 环境。至于怎么做，网上有好多教程。我用的是 mac ，步骤如下：

- <pre class="prettyprint">sudo gem update --system</pre>
- <pre class="prettyprint">sudo gem install rdiscount</pre>
- <pre class="prettyprint">easy_install Pygments</pre>

这样就可以在本地调试了，在你的网站目录下执行
<pre class="prettyprint">jekyll --pygments --server</pre>

PS：最简单的放一个 _config.yml 文件和一个 index.html 就可以让网站 run 了，先了解一下一套完整的 jekyll 的[文件结构和配置][] 是必须的。

## 导入 wordpress 文章

换了新的 blog ，最麻烦和头疼的就是旧数据的导入，最起码文章要全部要保留。jekyll 提供了导入方法，首先找到 jekyll 安装的路径，把migrators/wordpress.rb
拷贝出来执行，下面是我本地的路径和执行方法：
<pre class="prettyprint">
/Library/Ruby/Gems/1.8/gems/jekyll-0.11.2/lib/jekyll/migrators
ruby -r wordpress.rb -e 'Jekyll::WordPress.process("wordpressDB", "root", "root")'
</pre>

执行后，会生成一个 _post 文件夹，里面就是文章了，直接 copy 到自己的网站目录 `_post/` 即可。

PS：远程连接wordpress数据库可能很慢，容易中途出错而失败，最好先把数据库拷贝到本地 mysql ，在执行就没问题了。

## 关于 CNAME

关于配置 CNAME ，两种情况：

- 用二级域名如：blog.luchanghong.com ，那么就去域名管理界面增加一个 CNAME <pre>blog.luchanghong.com -> luchanghong.github.com</pre>
此时网站根目录下的 CNAME 文件填写 `blog.luchanghong.com`

- 用顶级域名如：luchanghong.com ，那么就把原来域名解析 A 记录指定的 IP 地址修改为 `204.232.175.78` ，为了使 `www.luchanghong.com` 也生效，
增加一个 CNAME <pre>www.luchanghong.com -> luchanghong.com</pre>

PS：可以参考 github pages 的 CNAME [帮助文档][]







[github]: http://www.github.com "github"
[这里]: https://github.com/mojombo/jekyll/wiki/sites "这里"
[jekyll]: http://baike.baidu.com/view/7878719.htm "jekyll"
[文件结构和配置]: https://github.com/mojombo/jekyll/wiki/usage "文件结构和配置"
[帮助文档]: https://help.github.com/articles/my-custom-domain-isn-t-working "帮助文档"
