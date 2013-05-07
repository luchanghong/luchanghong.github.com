--- 
wordpress_id: 513
wordpress_url: http://luchanghong.com/rosemary/?p=513
date: 2012-08-23 12:59:12 +08:00
layout: post
title: mac下ssh自动登陆
category: linux_OSX
tags: [ssh, mac, linux]
description: 如果你有很多台服务器要维护，频繁的输入口令肯定会让你厌烦。 SSH 通过证书（公钥、密钥对）来登陆服务器为我们 解决了这个问题，不用每次都输入那些复杂而且看不见的密码了。
---
每次登陆服务器都要输入密码，很烦人啊，要是一台服务器还凑合，我天天至少登陆4台，来回切换操作甚是麻烦，所以就做个自动登陆的吧……

## 一、创建一对公钥、密钥

```bash
ssh-keygen -t rsa
```
或者

```bash
ssh-keygen -t dsa
```

然后会提示生成路径以及让你输入密码：

```bash
lchmatoMacBook-Pro:.ssh lch$ ssh-keygen -t dsa
Generating public/private dsa key pair.
Enter file in which to save the key (/Users/lch/.ssh/id_dsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

提示：这里输入的密码是保存在本地的密码，不是你要登陆服务器的密码，目前的操作和服务器无关，<span style="color: #ff0000;">推荐不输入密码</span>

## 二、把公钥拷贝到服务器

把生成的id_dsa.pub或者id_rsa.pub内容拷贝到服务器.ssh/authorized_keys这个文件里，如果没有就创建，如果存在，就另起一行添加，因为这里也可能有别人的公钥

## 三、OVER

上面操作结束，再次登陆服务器就不用输密码了
