---
layout: post
title: Linux下创建用户并配置FTP让其访问
category: server
tags: [linux]
description: Linux下创建用户是很easy的事情了，只不过不经常去做这些操作，时间久了就容易忘记，顺便配置一下FTP。
---

**声明** 使用Linux版本`CentOS release 5.6`，并以超级管理员`root`身份运行

## 创建新用户

1.创建用户，并指定分组和主目录

    useradd -d /home/lch -g root lch

 还可以增加其他参数，比如指定用户使用shell等，具体的google一下

2.设定密码 

    passwd lch

3.查看、改变、添加用户分组

    [root@localhost ~]# groups lch
    lch : root www
    # -G 改变分组
    [root@localhost ~]# usermod -G root lch
    [root@localhost ~]# groups lch
    lch : root
    # -g 新增分组
    [root@localhost ~]# usermod -g www lch
    [root@localhost ~]# groups lch
    lch : www root

4.删除用户

    # 加上 -r 参数，删除更彻底
    userdel -r lch

## 更改ftp配置文件

   add lch

修改配置文件`/etc/vsftpd/vsftpd.conf`并设定或删掉注释：

    userlist_enable=NO
    anonymous_enable=NO

    chroot_list_enable=YES
    chroot_list_file=/etc/vsftpd/chroot_list

打开`/etc/vsftpd/user_list`并增加一行：

    lch

新建文件`/etc/vsftpd/chroot_list`并增加一行

    lch

## 启动FTP服务

    service vsftpd start

还有两个参数：stop、restart
