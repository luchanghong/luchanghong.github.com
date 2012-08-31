---
date: 2012-08-27 17:35:54 +08:00
title: macBook技巧集锦
layout: post
---
MacBook Pro使用

我的MacBook：Mac OS X 10.7.3

1.获取root权限

?
1
sudo passwd root
会提示输入当前账户密码，然后输入两次新的root账户密码

切换到root，su命令，然后输入root账户密码，退出root用exit

2.安装Xcode之后，编译C还需要安装Command Line Tools

进入Xcode -> Preference -> Downloads -> Command Line Tools，点选install

3.添加环境变量

?
1
sudo vi /etc/paths
