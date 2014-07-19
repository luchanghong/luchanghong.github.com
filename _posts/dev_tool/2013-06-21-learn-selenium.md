---
layout: post
category: dev_tool
title: WEB自动化测试工具Selenium
tags: [svn]
description: 尽管做开发，了解一下测试相关的东西也好。Selenium早有耳闻，却一直没认真看过，先简单学习一下。
---

## Selenium

[Selenium][1]是一个自动化WEB测试工具，支持Chrome、Firefox、Safari、IE等多种常用的浏览器。

[1]: http://www.seleniumhq.org "selenium"

## selenium Project

[Selenium Project][2]也就是一些常用的测试工具，下面按照官网一一介绍。

[2]: http://www.seleniumhq.org/projects "selenium projects"

### Selenium IDE

这是一个Firefox的插件，显然只适用于Firefox浏览器，这里就不做介绍了，可以参考官网[Selenuim IDE][3]。

[3]: http://www.seleniumhq.org/projects/ide "selenium ide"

### Selenium Remote Control

一个C/S系统，简称`Selenium RC`，可以允许操作客户端（本地或者其他计算机）的web browser。使用步骤如下：

- 下载并安装Selenium Server，最新[2.33下载地址][download selenium server]。实际上这是一个jar包，不用安装，只需要运行：

    ```bash
    # 首先要确认安装了java，并且版本在1.5上
    lch@localhost:selenium $ java -version
    java version "1.6.0_45"
    Java(TM) SE Runtime Environment (build 1.6.0_45-b06-451-11M4406)
    Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01-451, mixed mode)
    # 运行selenium server
    lch@localhost:selenium $ java -jar selenium-server-standalone-2.33.0.jar
    2013-6-20 12:32:29 org.openqa.grid.selenium.GridLauncher main
    ��Ϣ: Launching a standalone server
    12:32:34.307 INFO - Java: Apple Inc. 20.45-b01-451
    12:32:34.307 INFO - OS: Mac OS X 10.8.4 x86_64
    12:32:34.318 INFO - v2.33.0, with Core v2.33.0. Built from revision 4e90c97
    12:32:34.422 INFO - RemoteWebDriver instances should connect to: http://127.0.0.1:4444/wd/hub
    12:32:34.423 INFO - Version Jetty/5.1.x
    12:32:34.424 INFO - Started HttpContext[/selenium-server/driver,/selenium-server/driver]
    12:32:34.425 INFO - Started HttpContext[/selenium-server,/selenium-server]
    12:32:34.425 INFO - Started HttpContext[/,/]
    12:32:34.464 INFO - Started org.openqa.jetty.jetty.servlet.ServletHandler@21b64e6a
    12:32:34.464 INFO - Started HttpContext[/wd,/wd]
    12:32:34.470 INFO - Started SocketListener on 0.0.0.0:4444
    12:32:34.471 INFO - Started org.openqa.jetty.jetty.Server@21ec6696
    ```

[download selenium server]: http://selenium.googlecode.com/files/selenium-server-standalone-2.33.0.jar "ownload selenium server"

- 下载并安装[客户端驱动][client driver]。客户端支持java、csharp(c#)、python、ruby、php、perl、javascript等语言，选择对应的驱动包即可。这里我使用python作为我的客户端编程语言。

    ```bash
    # 使用python的话可以用pip来安装
    pip install selenium
    ```

    测试代码借鉴官网上的，利用了python的unitest库，如下：

    ```python
    from selenium import selenium
    import unittest, time, re

    class NewTest(unittest.TestCase):
        def setUp(self):
            self.verificationErrors = []
            self.selenium = selenium("localhost", 4444, "*firefox",
                    "http://www.google.com/")
            self.selenium.start()

        def test_new(self):
            sel = self.selenium
            sel.open("/")
            sel.type("q", "selenium rc")
            sel.click("btnG")
            sel.wait_for_page_to_load("30000")
            self.failUnless(sel.is_text_present("Results * for selenium rc"))

        def tearDown(self):
            self.selenium.stop()
            self.assertEqual([], self.verificationErrors)

    if __name__ == '__main__':
        unittest.main()
    ```

    执行这个脚本就能看到效果了。

关于Selenium RC的介绍就到这里了，更具体的应用可以参考官网[Selenium RC][selenium rc]。

[client driver]: http://www.seleniumhq.org/docs/05_selenium_rc.jsp#using-the-java-client-driver "java client driver"
[selenium rc]: http://www.seleniumhq.org/docs/05_selenium_rc.jsp "selenium rc"

### Selenium WebDriver

WebDriver可以驱动本地或者远程计算机上的web browser。它可以独立于Selenium RC进行浏览器模拟测试，具体的介绍可以参考官网[Selenium WebDriver][selenium webdriver]。下面请看简单用法：

```python
>>> from selenium import webdriver
>>> driver = webdriver.Chrome()
chromedriver
>>> driver.get('http://www.baidu.com')
```

在默认情况下，chrmoedriver并不在当前的PATH中，这会出错：

```python
>>> driver = webdriver.Chrome()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/lch/.pythonbrew/pythons/Python-2.7.3/lib/python2.7/site-packages/selenium/webdriver/chrome/webdriver.py", line 59, in __init__
    self.service.start()
  File "/Users/lch/.pythonbrew/pythons/Python-2.7.3/lib/python2.7/site-packages/selenium/webdriver/chrome/service.py", line 68, in start
    Please download from http://code.google.com/p/selenium/downloads/list\
selenium.common.exceptions.WebDriverException: Message: 'ChromeDriver executable needs to be available in the path.                 Please download from http://code.google.com/p/selenium/downloads/list                and read up at http://code.google.com/p/selenium/wiki/ChromeDriver'
```

此时需要做一个软链接即可：

```bash
lch@localhost:Desktop $ ls -s /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome /usr/local/bin/chromedriver
```

[selenium webdriver]: http://docs.seleniumhq.org/docs/03_webdriver.jsp "selenium webdriver"

### Selenium Grid

利用Selenium Remote Control同时完成许多不同的测试项目
