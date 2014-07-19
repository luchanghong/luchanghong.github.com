---
layout: post
title: 服务器配置和LAMP环境搭建
category: server
tags: [lamp, php, nginx, mysql]
description: 13年下半年确实是忙碌，应用迭代开发速度极快，一版接着一版。最近服务器要迁移到云平台，顺便做一下整个过程的记录以备后用。
---

## Nice - 品牌滤镜

这是我们公司开发的APP的名字（之前叫KK购物），看着高大上吧，这里就纯粹做一个广告，欢迎下载，目前只有[iOS版][ios-download]。

[ios-download]: https://itunes.apple.com/cn/app/kk-gou-wu/id641895599?mt=8 "nice - 品牌滤镜"

## 配置信息

我们选择的是[青云][qing-cloud]这家年轻的云计算服务平台，和我们一样也是创业。下面是硬件配置清单：

- 硬件
    * CPU：4核
    * 内存：16G
    * 带宽：5M
    * 硬盘：500G

- 软件
    * OS：CentOS 6.4 64bit
    * WEB Server：nginx/1.0.15
    * Program Language：PHP 5.3.3 Zend Engine v2.3.0
    * Database：mysql Ver 14.14 Distrib 5.1.71
    * Cacahe：Redis server version 2.4.10

[qing-cloud]: https://www.qingcloud.com "青云 Qing Cloud"

## 安装步骤

略过在青云控制台的一些配置操作，进入主题：

- 挂载硬盘，把500G格式化分区后挂载到/data目录下
    * fdisk -l
    * fdisk /dev/sdc
    * p：查看 n：新分区 w：保存并退出
    * mkfs.ext3 /dev/sdc1
    * mount /dev/sdc1 /data

- 配置防火墙
    * 开放22端口以供SSH服务使用
    * 开放8080端口供测试访问
    * 其他

- 软件安装，使用自带的`yum`来安装
    * yum install nginx
    * yum install php
    * yum install php-fpm
    * yum install mysql
    * yum install mysql-server (mysql启动管理）
    * yum install php-mysql（安装mysql扩展）
    * yum install redis

- 修改各软件的配置文件
    * 所有的日志路径，统一在/data/var/log
    * php.ini和php-fpm.conf配置
    * nginx.conf配置，每个站点单独一个配置文件
    * my.ini配置，设置数据库各个参数和数据库目录
    * redis.conf配置，配置redis数据库路径

- 启动相应的服务，并做检查
    * /etc/init.d/mysqld start
    * /etc/init.d/php-fpm start
    * /etc/init.d/redis start
    * /etc/init.d/nginx start
    * ps aux | grep process_name来check

其实这些东西都是很简单的，只不过我们平时练习的少，大部分时间还是专注在开发上。

## 注意的问题

由于新服务器和老服务器环境差别大，可能会忘记安装一些软件或者第三方类库以及PHP扩展
等，这样都会出现很多令人费解的问题。

所以，这种情况下出现问题首先要考虑到软件实施上的问题，先把代码抛到一边。
