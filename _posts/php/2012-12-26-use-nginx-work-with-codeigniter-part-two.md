---
layout: post
title: Nginx配置CodeIgniter项目（二）
category: php
tag: [php,odeigniter]
description: 两种Nginx部署CI项目的方法，一个是rewrite，另一个是用PATH_INFO模式，当然是后者比较简单。
---

# URL rewrite 方法

```nginx
server {
    listen 8080;
    server_name www.xxx.com;

    root /Users/lch/work/www/ci;

    access_log /usr/local/var/log/access.log;
    error_log /usr/local/var/log/error.log;

    location ~ ^/(img|images|script|js|css|upload)/ {
        root /Users/lch/work/www/ci;
        break;
    }

    location ~ { 
        if (!-e $request_filename) {
            # for /admin
            rewrite ^/(admin)$ /index.php?c=welcome&m=index&d=$1 break;
            # for /admin/index
            rewrite ^/(admin)/([a-zA-Z_]+)$ /index.php?c=$2&m=index&d=$1 break;
            # for /admin/account/login
            rewrite ^/(admin+)/([a-zA-Z_]+)/([a-zA-Z_]+)$ /index.php?c=$2&m=$3&d=$1 break;
                
            ## for general URL
            rewrite ^/([a-zA-Z_]+)/([a-zA-Z_]+)/?(.*)$ /index.php?c=$1&m=$2 last;
        } 

        root /Users/lch/work/www/ci;
        fastcgi_pass 127.0.0.1:9001;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

说明一下上面的配置，因为我在`application/controllers/`里面新建一个文件夹`admin`专门存放后台相关的controller，所以比普通的路径要多一层（对应的是&d=admin）这个参数。在这就可以看到rewrite方法的不足了，当有类似admin这种情况发生的时候就要添加对应的rewrite规则了。

## PATH_INFO 方法

```nginx
server {
    listen 8080;
    server_name www.xxx.com;

    root /Users/lch/work/kidulty/snap_www;

    access_log /usr/local/var/log/snap_access.log;
    error_log /usr/local/var/log/snap_error.log;

    location ~ ^/(img|images|script|js|css|upload)/ {
        root /Users/lch/work/kidulty/snap_www;
        break;
    }

    if (!-e $request_filename) {
        rewrite ^(.*)$ /index.php/$1 last;
    }   

    location ~ { 
        set $path_info ""; 
        set $real_script_name $fastcgi_script_name;
        if ($fastcgi_script_name ~ "^(.+?\.php)(/.+)$") {
            set $real_script_name $1;
            set $path_info $2;
        }

        root /Users/lch/work/kidulty/snap_www;
        fastcgi_pass 127.0.0.1:9001;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$real_script_name;
        fastcgi_param SCRIPT_NAME $real_script_name;
        fastcgi_param PATH_INFO $path_info;
        include fastcgi_params;
    }
}
```

说明：

* 如果项目里面URL类似`http://www.xxx.com/index.php/user/profile`这种就不用下面的rewrite：

```nginx
if (!-e $request_filename) {
    rewrite ^(.*)$ /index.php/$1 last;
}
```

* 注意本机fastcgi启动的是9001端口，要和自身匹配
* 网上有人说`include fastcgi_params;`要放在设置`fastcgi_param`的前面，经测试前后都没问题
