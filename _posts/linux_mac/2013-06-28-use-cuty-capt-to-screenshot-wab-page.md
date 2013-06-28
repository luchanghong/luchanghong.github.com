---
layout: post
title: CutyCapt——命令行下的网页截屏工具
tags: [cutycapt]
category: linux_mac
description: 最近被项目一个需求弄得头大，把不同数据生成的网页截取下来然后分享到微博。翻来覆去好久终于找到了这一款很实用的软件——CutyCapt。
---

## CutyCapt

[CutyCapt][1]这个项目是基于Qt的，托管在sourceforge上，相关介绍请移驾浏览，但是sourceforge有时会被墙。

[1]: http://cutycapt.sourceforge.net

## Install

CutyCapt依赖`Qt4.4.0+`，所以安装之前要确保Qt相关的库被安装。在我的Mac Pro上用homebrew安装的，相关的依赖都自动安装好了：

```bash
brew install cuty_capt
```

但是在服务器上安装的时候着实麻烦，现在服务器上也没装好，只能在本地把图片生成出来再传到服务器上去了，幸亏图不多！

## Usage

- shell下使用

    直接敲命令，先呼出`help`看看参数
    
    ```bash
    CutyCapt --private-browsing=off --min-width=440 --min-height=440 
    --url="http://192.168.1.109:8080/genpic/gen_wb_shar_img/6?s=88.4&s1=1&s2=0.5&s3=0.88&s4=1&s5=0.82" --out=a.png
    ```

- PHP/Python等编程语言中使用

    其实就是调用系统的shell命令，比如：
    
    ```
    # PHP
    $url = '';
    $file = '';
    exec("/usr/local/bin/CutyCapt --min-width=400 --min-height=400 --url=\"{$url}\" --out={$file}");

    # Python
    >>> os.system('CutyCapt --private-browsing=off --min-width=440 --min-height=440 
    --url="http://192.168.1.109:8080/genpic/gen_wb_shar_img/6?s=88.4&s1=1&s2=0.5&s3=0.88&s4=1&s5=0.82" --out=aa.png')
    ```

下面附上`CutyCapt.sourceforge.net`的截图：

![Alt cutycapt.sourceforge.net](/upload/2013/06/cutycapt-cscreenshot.png)

