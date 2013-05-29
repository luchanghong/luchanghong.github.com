---
layout: post
title: Install PIL(Python Image Library) on Mac OS X
catrgory: linux-mac
tags: [python, pil]
description: 通常直接通过easy_install或者pip安装PIL后并不支持JPEG，在使用的过程中会出现“decoder jpeg not available”这样的警告，这时候需要手动编译安装PIL才能解决问题。
---

## 安装JPEGLIB

在[ijg.org][1]上下载一个较新版本的JPEG库，然后解压、编译、安装。

```bash
tar -xvzf jpegsrc.v8c.tar.gz
cd jpeg-8c
./configure
make
sudo make install
```

安装完成之后找到libjpeg.8.dylib所在的路径，我的是：/usr/local/lib/libjpeg.8.dylib，这个很重要，为后面编译安装PIL做准备。

[1]: http://www.ijg.org/files

## 安装PIL

在[pythonware.com][2]上下载PIL，解压后编译、安装。

- 编辑setup.py，定义`JPEG_ROOT`，这就是上一步中的libjpeg.8.dylib所在路径。

    ```python
    # line 37
    JPEG_ROOT = "/usr/local/lib"
    ```

- 编译

    ```bash
    python setup.py build_ext -i
    ```

- 测试

    ```bash
    (env_lbew)lch@Mac-Pro-LCH:Imaging-1.1.7 $ python selftest.py
    --------------------------------------------------------------------
    PIL 1.1.7 TEST SUMMARY
    --------------------------------------------------------------------
    Python modules loaded from ./PIL
    Binary modules loaded from ./PIL
    --------------------------------------------------------------------
    --- PIL CORE support ok
    --- TKINTER support ok
    --- JPEG support ok
    --- ZLIB (PNG/ZIP) support ok
    --- FREETYPE2 support ok
    *** LITTLECMS support not installed
    --------------------------------------------------------------------
    Running selftest:
    --- 57 tests passed.
    ```

    `--- JPEG support ok`说明已经成功了。

- 安装

    ```bash
    python setup.py Install
    ```

[2]: http://www.pythonware.com/products/pil

## 总结

安装的过程要比看完这一篇文章复杂漫长的多，而且不同系统也会遇到各种各样的问题，但是问题中就是能解决的。
