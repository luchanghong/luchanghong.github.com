---
layout: post
title: PHP类中的静态方法
tags: [php, class]
category: php
description: 一般在WEB应用里，我们很难用到类的一些高级（或者称独特）的用法，来一起学习一下静态方法的使用。
---

## 静态方法定义

定义静态方法很简单，在声明关键词`function`之前加上`static`，例如：

```php
class A
{
    static function fun()
    {
        // do somathing
    }
}
```

## 静态方法使用

使用的时候和静态变量差不多，不需要实例化，直接用`::`调用，例如：

    A::fun()

## 对比普通方法

因为静态方法的调用不需要实例化，所以在静态方法中引用类自身的属性或者方法的时候会出错，也就是形如`self`和`$this`是错误的。

    class MyClass
    {
        public $num = 5;

        function __construct()
        {
            $this->num = 10;
        }

        function fun_1()
        {
            echo "I am a public method named fun_1.\n";
            echo "The num of object is {$this->num}.\n";
        }

        static function fun_2()
        {
            echo "I am a static method named fun_2.\n";
        }

        function fun_3($n)
        {
            echo "The arg is {$n}\n";
        }
    }

    $m = new MyClass;
    $m->fun_1();
    $m->fun_2();
    $m->fun_3('test');

    MyClass::fun_1();
    MyClass::fun_2();
    MyClass::fun_3('test');

输出结果：

    lch@localhost:php $ php class_method.php
    I am a public method named fun_1.
    The num of object is 10.
    I am a static method named fun_2.
    The arg is test
    I am a public method named fun_1.
    PHP Fatal error:  Using $this when not in object context in /Users/lch/program/php/class_method.php on line 14
