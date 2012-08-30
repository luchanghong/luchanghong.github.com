--- 
wordpress_url: http://luchanghong.com/rosemary/?p=454
wordpress_id: 454
title: "error: freetype/config/ftheader.h: No such file or directory "
layout: post
date: 2012-07-24 17:39:07 +08:00
---
安装captchaimage这个package的时候，总是编译不成功，修改指定编译的路径之后又报了下面的错误：
<pre>[shell]
captchaimage.c: In function ‘captchaimage_create_image’:

captchaimage.c:297: error: ‘FT_Library’ undeclared (first use in this function)
captchaimage.c:297: error: (Each undeclared identifier is reported only once
captchaimage.c:297: error: for each function it appears in.)

captchaimage.c:297: error: expected ‘;’ before ‘library’
captchaimage.c:311: warning: implicit declaration of function ‘FT_Init_FreeType’
captchaimage.c:311: error: ‘library’ undeclared (first use in this function)

captchaimage.c:316: warning: implicit declaration of function ‘create_image_internal’

captchaimage.c:317: warning: initialization makes pointer from integer without a cast

captchaimage.c:318: warning: implicit declaration of function ‘FT_Done_FreeType’

error: command 'gcc' failed with exit status 1

----------------------------------------
Command /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_push/bin/python -c "import setuptools;__file__='/Users/lch/.pythonbrew/venvs/Python-2.6.7/env_push/build/captchaimage/setup.py';exec(compile(open(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" install --single-version-externally-managed --record /var/folders/p0/yvfn32cx5hncxjdcsgb6bq940000gn/T/pip-PrV94o-record/install-record.txt --install-headers /Users/lch/.pythonbrew/venvs/Python-2.6.7/env_push/include/site/python2.6 failed with error code 1
[/shell]</pre>
去网上搜素“error: command 'gcc' failed with exit status 1”，我的gcc也没什么问题，找不到解决问题的办法，好像好多错误最终都会报这点。再仔细看log，发现：

[shell]error: freetype/config/ftheader.h: No such file or directory [/shell]

在/usr/X11/include下面发现fretype2，原来在里面一层才是freetype，于是做个link

[shell]sudo ln -s /usr/X11/include/freetype2/freetype/ /usr/X11/include/freetype[/shell]

再次安装，OK了

[shell]pip install captchaimage[/shell]

&nbsp;
