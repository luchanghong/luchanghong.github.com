---
layout: post
title: 使用codeIgniter框架
tags: [php, codeigniter]
category: php
description: 做回老本行了，第一次使用 codeIgniter 框架。
---

## 下载安装
    
直接去 [codeigniter 中文官网][]下载，解压即可。最好单独配置一个独立虚拟域名做测试，我配置的是cifw.com，意思就是 codeIgniter Framework 。

## 开始使用

- URL重写

如果你不想把入口文件`index.php`放在URL中，那么就简单做一下Apache Rewrite：

    RewriteEngine on
    RewriteCond $1 !^(index\.php|images|robots\.txt)
    RewriteRule ^(.*)$ /index.php/$1 [L] 

这样就会优化URL，原来的：
    http://cifw.com/index.php/hello/add
将变成：
    http://cifw.com/hello/add

- 匹配URL路由
    
<pre class="prettyprint">
# My test URI
$route['hello/index'] = 'hello/index';
$route['hello/(:any)'] = 'hello/any';
</pre>

- 创建一个controller

在`/application/controllers`目录下创建hello.php，创建`Hello`类并继承`CI_Controller`。

- 使用view模板

<pre class="prettyprint">
$this->load->view('hello_any');
</pre>

在`/application/views`目录下创建 **hello_any.php** 即可。

- 向模板传递变量

把变量存入数组中，在模板中以键值作为变量名引用，比如：

<pre class="prettyprint">
public function menu()
{   
    $data = array(
        'menu' => array(
            'message list' => '/hello/list',
            'message add'  => '/hello/add',
        ),  
    );  
    $this->load->view('hello_menu', $data);
}  
</pre>

在模板中直接以 `$menu` 来使用即可。循环输出可以参考：

    <?php foreach($menu as $key=>$val):?>
        <?=$val;?> <?=$key;?>
    <?php endforeach;?>

这里使用了CI自带的模板语法，要在`/application/config/config.php`中开启：

<pre class="prettyprint">
$config['rewrite_short_tags'] = TRUE;
</pre>
    
- 创建一个数据库Model

在`/application/models`目录下创建hello_model.php，创建`Hello_model`并继承`CI_Model`，例如：

<pre class="prettyprint">
class Hello_model extends CI_Model {

    var $uname     = ''; 
    var $email     = ''; 

    function __construct()
    {   
        parent::__construct();
    }   

    function insert_message()
    {   
        $this->uname = $this->input->post('uname');
        $this->email = $this->input->post('email');
        $this->db->insert('message', $this);
        // echo 'ok';
    }   
}
</pre>

- 使用网页缓存

在要使用缓存的function里面加上如下声明，1表示1分钟。
<pre class="prettyprint">
$this->output->cache(1); 
</pre>


[codeigniter 中文官网]: codeigniter.org.cn "codeigniter.org.cn"
