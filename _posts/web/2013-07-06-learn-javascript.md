---
layout: post
category: web
title: Learn JavaScript
tags: [javascript]
description: JavaScript学习概述。声明：例子都是在node.js的shell环境运行的，所以出现好多undefined行，阅读时请主动忽略。
---

# 目录

- [简介](#introduction)

    - [发展历史](#history)

        - [Nombas和ScriptEase](#NombasAndScriptEase)
        - [Netscape发明了JavaScript](#NetscapeCreateJavaScript)
        - [三足鼎立](#threeJavaScripts)
        - [标准化](#JavaScriptISO)

    - [JavaScript是什么](#whatIsJavaScript)
    
        - [组成](#JavaScriptCompose)
        - [ECMAScript](#ECMAScript)
        - [DOM](#DOM)
        - [BOM](#BOM)

- [基础](#base)

    - [语法](#syntax)

        - [弱类型](#weakly)
        - [区分大小写](#caseSensitive)
        - [块标记和结尾](#blockMark)
        - [注释](#annotation)

    - [变量](#variables)

        - [变量声明](#declare)
        - [变量命名](#named)
        - [关键字和保留字](#keywords)

    - [数据类型](#dataType)

        - [原始和引用类型](#primitiveAndReferenceType)
        - [typeof运算符](#typeof)
        - [类型转换](#typeConversion)

- [运算符](#operator)

    - [赋值运算符](#assignment)
    - [一元运算符](#unary)
    - [加减乘除运算符](#arithmetic)
    - [Boolean运算符](#booleanOperator)
    - [关系运算符](#relationship)
    - [等性运算符](#equality)
    - [条件运算符](#conditional)
    - [位运算符](#bitwise)

- [语句](#statement)

    - [if语句](#if)
    - [switch语句](#switch)
    - [循环语句](#iterative)
    - [break和continue语句](#breakAndContinue)

- [函数](#function)

    - [函数概述](#functionConcept)
    - [arguments对象](#arguments)
    - [Function对象](#functionObject)
    - [closure闭包](#closure)

- [对象和类](#objectAndClass)

    - [面向对象](#objectOriented)
    - [对象类型](#objectTypes)
    - [对象作用域](#objectScope)
    - [创建对象](#createObject)
    - [修改对象](#modifyObject)
    - [使用对象](#useObject)

___

# <a id="introduction"></a> 简介

## <a id="history"></a> 发展历史

### <a id="NombasAndScriptEase"></a> Nombas和ScriptEase

1992年，Nombas公司开发一种命名为`C-minus-minus`的嵌入式脚本语言，简称`Cmm`，后
改名为`ScriptEase`，因为`Cmm`的后面两个字母比较消极。

### <a id="NetscapeCreateJavaScript"></a> Netscape发明JavaScript

1995年，Netscape为即将发行的`Netscape Navigation 2.0`开发一个脚本语言命名为
`LiveScript`，并与Sun公司合作。在正式发布之前，Netscape将其更名为`JavaScript`，目
的是想借助Sun公司的`Java`语言火起来。

### <a id="threeJavaScripts"></a> 三足鼎立

微软在IE3.0版本中搭载了一个`JavaScript`的克隆版，命名为`JScript`。从此Netscape的
JavaScript、MicroSoft的JScript和Nombas的ScriptEase这三种不同的版本同时存在。

### <a id="JavaScriptISO"></a> 标准化

1997年，由欧洲计算机制造协会（ECMA）主持，来自Netscape、微软、Borland和一些对脚本
语言感兴趣的公司程序员一起参与并最终制定`ECMAScript`为`JavaScript`实现的标准。

## <a id="whatIsJavaScript"></a> JavaScript 是什么

### <a id="JavaScriptCompose"></a> 组成

`ECMAScript`是`JavaScript`的执行标准，但不是唯一的部分。完整的`JavaScript`包含三
部分：

- 核心（ECMAScript）
- 文档对象模型（DOM）
- 浏览器对象模型（BOM）

### <a id="ECMAScript"></a> ECMAScript

`ECMAScript`不与任何浏览器绑定，也没有指定输入和输出的方法，下面是`ECMA-262`标准
的描述：

> ECMAScript 可以为不同种类的宿主环境提供核心的脚本编程能力，因此核心的脚本语言
是与任何特定的宿主环境分开进行规定的...

浏览器（browser）对于`ECMAScript`来说就是一个宿主环境。`ECMAScript`描述了以下内
容：

- 语法
- 类型
- 语句
- 关键字
- 保留字
- 运算符
- 对象

`ECMAScript`只是定义脚本语言的以上属性规则，而`JavaScript`的实现就是按照这个作为
标准进行扩展。不仅仅是`JavaScript`，`JScript`、`ActionScript`等都是基于`ECMAScript`
扩展而来的。

### <a id="DOM"></a> DOM

DOM（Document Object Model）即为文本对象模型，是HTML和XML应用程序的接口。这是
为了规范页面结构，显然是针对当时Netscape和微软浏览器竞争而制定的，统一了DOM对象操
作（增删改查）的方法，实现WEB的跨平台。否则，在Windows平台下能正常浏览的网页却不
能正常显示在其他系统下。

DOM并不是JavaScript所特有的，它为JavaScript操作HTML页面各个节点提供了入口。

DOM包含两个部分：DOM Core和DOM HTML。前者定义了整个文档的结构图，以便访问和操作
节点；后者针对HTML扩展一些专用的对象和方法。另外，DOM包含3个level，具体信息可以
百科。

### <a id="BOM"></a> BOM

BOM（Browser Object Model）即为浏览器对象模型，提供了对浏览器操作（如移动窗口，
改变状态栏文本、打开新窗口等）的入口。

BOM只是`JavaScript`的一部分，且没有相关的标准。常用的BOM对象：

- window 对象
- navigator 对象
- screen 对象
- history 对象
- location 对象

___

# <a id="base"></a> 基础

## <a id="syntax"></a> 语法

### <a id="weakly"></a> 弱类型

`JavaScript`的变量类型属于弱类型，在定义变量的时候可以初始化任意值。但是在值改
变的情况下最好保持变量的类型。

### <a id="caseSensitive"></a> 区分大小写

`JavaScript`的变量是区分大小写的，变量a和A是不同的。

### <a id="blockMark"></a> 块标记和结尾

`JavaScript`的每一条语句结尾用分号`;`结束，为了方便阅读最好一行一条语句，但`;`
不是必须的（不像PHP）；

`JavaScript`的代码块用一对大括号`{ }`来划分。

### <a id="annotation"></a> 注释

`JavaScript`的注释分单行和多行。

- 单行用`//`
- 多行用`/* */`

## <a id="variables"></a> 变量

### <a id="declare"></a> 变量声明

`JavaScript`声明变量用`var`。

```javascript
// 定义变量name
var name = 'luchanghong';
// 同时定义两个变量
var age = 25, gender = 'male';
// 定义一个变量，但不赋值
var a;
```

### <a id="named"></a> 变量命名

`JavaScript`的变量命名规则有两条：

- 必须以字母、下划线(_)或者美元符号($)开头
- 第二位开始可以是字母、数字、下划线或者美元符号

```javascript
var name;
var $name;
var _name;
var __name;
var name_2;
```

这里不得不提两种常用的命名规则：

- camel标记法

    驼峰命名法，首字母小写，接下来每个单词首字母大写。

    ```javascript
    var myFirstName = 'changhong';
    ```

- pascal标记法

    帕斯卡命名法，首字母大写，接下来每个单词首字母也大写。

    ```javascript
    var MyLastName = 'lu';
    ```

### <a id="keywords"></a> 关键字和保留字

`JavaScript`里有一套关键字，标志着语句的开始或者结尾，这些关键字不能作为函数或
者变量的名称。

所谓的保留字是为了将来作为关键字而预留的，也不能作为函数或者变量的名称。

## <a id="dataType"></a> 数据类型

### <a id="primitiveAndReferenceType"></a> 原始和引用类型

`JavaScript`的变量存在两种类型的值：

- 原始值：存储在栈（stack）中的简单数据。值直接存放在变量访问地址的位置。

- 引用值：存储在堆（heap）中的对象。变量访问的位置存储的的是值，而是指针（point
），指向对象的存储地址。

原始值有5种类型（原始类型）：Number、String、Boolean、Null和Undefined。

引用值对应的类型（引用类型）通常就是对象（Object）。

### <a id="typeof"></a> typeof运算符

typeof用来检测变量的类型。

```javascript
lch@localhost:web $ node
> typeof 123
'number'
> typeof '123'
'string'
> typeof true
'boolean'
> typeof a
'undefined'
> typeof Object()
'object'
> typeof null
'object'
```

Null被认为是空对象的占位符，所以类型也是Object。

### <a id="typeConversion"></a> 类型转换

类型转换是程序经常用到的，这也是编程语言设计的重要部分。

- 转换成字符串

    3中主要的原始类型String、Number和Boolea都有`toString()`方法，可以把值转换成
    字符串。

    数字转换成字符串是比较有意思的，可以强制转换进制，看下面例子：

    ```javascript
    // 整形转换成字符串
    > var n = 123;
    undefined
    > n.toString()
    '123'
    > n.toString(2)
    '1111011'
    > n.toString(16)
    '7b'
    // 浮点型转换成字符串
    > var m = 123.09
    undefined
    > m.toString()
    '123.09'
    > var p = 123.00
    undefined
    > p.toString()
    '123'
    // 布尔型转换成字符串
    > var b = true
    undefined
    > b.toString()
    'true'
    ```

    当然，可以用类似String(123)这样强制类型转换。

- 转换成数字

    内置两种方法：parseInt()和parseFloat()。
    
    ```javascript
    > parseInt('abc')
    NaN
    > parseInt('1abc')
    1
    > parseInt('1.4')
    1
    > parseInt('1.9')
    1
    // 0x表示16进制
    > parseInt('0xF')
    15
    // 0表示8进制
    > parseInt('010')
    8
    // 第二个参数是进制数
    > parseInt('010', 10)
    10
    ```

    注意的是，有前导0的时候加上第二个参数或者去掉前导0，以免当着8进制数转换了。

    ```javascript
    > parseFloat('123.9abc')
    123.9
    > parseFloat('0x12')
    0
    > parseFloat('abc')
    NaN
    > parseFloat('1.1.2')
    1.1
    ```

    parseFloat只对10进制数起作用，所以0x12就被当着普通字符串处理了。

- 强制类型转换

    上面提到String()强制类型转换，同样也有Number()和Boolean()。具体用法很简单，
    不再举例赘述。

___

# <a id="operator"></a> 运算符

## <a id="assignment"></a> 赋值运算

简单的赋值运算是通过等号（=）实现的，把右边的值赋予左边的变量。

复合赋值运算是把加减乘除等运算结合赋值运算而来，一般是简写，例如：

```javascript
> var n = 10;
undefined
> n = n + 10
20
> n += 10;
30
```

每种算术运算以及其他的几种运算都可以和赋值运算组合成复合赋值运算：

- 乘法/赋值（*=）
- 除法/赋值（/=）
- 取模/赋值（%=）
- 加法/赋值（+=）
- 减法/赋值（-=）
- 左移/赋值（<<=）
- 有符号右移/赋值（>>=）
- 无符号右移/赋值（>>>=）

## <a id="unary"></a> 一元运算符

一元运算符只有一个参数，即要操作的对象或值。

- 一元加减法

    一元加减法类似数学中的一元运算，一元加法不会产生影响，而一元减法会得到相反数。

    ```javascript
    > var m = 10,n = -10;
    undefined
    > console.log(+m,-m)
    10 -10
    undefined
    > console.log(+n,-n)
    -10 10
    Undefined
    ```

    另外，纯数字字符串也能进行一元加减法运算，得到的结果是Number类型，如：

    ```javascript
    > typeof -'-20'
    'number'
    ```

- 增量和减量运算

    通常在循环语句中使用（++/--）等运算。至于前、后的区别和其他编程语言是一致的：前
    增量/减量在实现的过程中会多一个中间变量，在前增量/减量运算完成后再赋值给该变量，
    而在此期间变量的值并不发生变化；后增量/减量运算则是在当时就改变了变量的值，所以
    不需要中间变量。

    ```javascript
    > var i = 1, j = 10;
    undefined
    > console.log(i++, ++j)
    1 11
    undefined
    > console.log(i, j)
    2 11
    undefined
    > console.log(i--, --j)
    2 10
    undefined
    > console.log(i, j)
    1 10
    Undefined
    ```

- delete

    delete用于删除已经定义的对象的属性或者方法。

    ```javascript
    > var MyInfo = new Object();
    undefined
    > MyInfo.name = 'luchanghong'
    'luchanghong'
    > MyInfo.printName = function() { console.log(this.name); return 'ok'; }
    [Function]
    > MyInfo.printName()
    luchanghong
    'ok'
    // 把name属性删除，当调用不存在的属性的时候，默认输出undefined
    > delete MyInfo.name
    true
    > MyInfo.printName()
    undefined
    'ok'
    ```

- void

    void运算符对任何值都返回undefined。一般在onclick事件触发AJAX数据交互的时候
    避免网页刷新或者跳转滚动时用到。

    ```html
    <a href="javascript:void(0)" onclick="doSomething()"> 查看更多 </a>
    ```

## <a id="arithmetic"></a> 加减乘除运算符

加法和减法都属于加性运算；乘法和除法都属于乘性运算。这些都是最简单的数学运算。

值得注意的是，字符串参与运算的时候要把数字转换成字符串连接起来，例如：

```javascript
> 5 + '4'
'54'
> 5 + 'a'
'5a'
> 5.4 + 'a'
'5.4a'
> 5.4 + '4'
'5.44'
```

### <a id="booleanOperator"></a> Boolean运算符

Boolean运算符在程序逻辑处理中是经常遇到的，有三种：AND、OR、NOT，也就是常说的与
或非。

- 各种类型转换成Boolean

    |   *参数类型*  |   *结果*  |
    |---------------|-----------|
    |   Undefined   |   false   |
    |   Null        |   false   |
    |   Boolean     |   结果等于输入的参数（不转换）|
    |   Number      |   如果参数为 +0, -0 或 NaN，则结果为 false；否则为 true |
    |   String      |   如果参数为空字符串，则结果为 false；否则为 true |
    |   Object      |   true    |

- NOT运算符

    非运算的符号是叹号（!），运算结果一定是Boolean类型。

    ```javascript
    > ! undefined
    true
    > ! null
    true
    > ! NaN
    true
    > ! 0
    true
    > ! -1
    false
    > ! 1
    false
    > ! 'a'
    false
    > ! Object()
    false
    ```

- AND运算符

    与运算的符号是（&&），运算结果可以是任意类型。对于特殊类型我统计了优先级：
    
    > NaN > null > undefined 

    ```
    > NaN && null
    NaN
    > null && undefined
    null
    > undefined && true
    undefined
    > undefined && false
    undefined
    > undefined && 1
    undefined
    > undefined && 'a'
    undefined
    > undefined && Object()
    undefined
    ```

    下面看常规类型：

    ```javascript
    // 忽略每行输出产生的undefined
    > console.log(1 && false, false && 1)
    false false
    undefined
    > console.log(1 && true, true && 1)
    true 1
    undefined
    > console.log(Object() && false, false && Object())
    false false
    undefined
    > console.log(Object() && true, true && Object())
    true {}
    undefined
    > console.log('a' && false, false && 'a')
    false false
    undefined
    > console.log('a' && true, true && 'a')
    true 'a'
    undefined
    > console.log(1 && Object(), Object && 1)
    {} 1
    ```

    发现规律了吧，如果把Number和String都理解为Object就更容易明白了。

- OR运算符

    或运算的符号是（||），其运算规律似乎和与运算相反，这里不做赘述了，自己动手
    感受吧。

## <a id="relationship"></a> 关系运算符

所谓的关系运算就是指比较运算，结果返回true或者false。常规的比较：大于、小于、
大于等于和小于等于。

```javascript
// 数字比较
> 1 > -1
true
// 字符串比较，取决首字母ASCII值
> 'a' > 'b'
false
> 'a' > 'B'
true
> '2' > '100'
true
// 数字和纯数字字符串
> 2 < '100'
true
// undefined比较
> 2 > undefined
false
> 2 < undefined
false
// null 比较
> 2 > null
true
> 2 < null
false
// NaN比较
> 2 > NaN
false
> 2 < NaN
false
```

## <a id="equality"></a> 等性运算符

除了上面的关系运算，等性运算也是程序设计中常遇到的。和其他编程语言一样，有两种
比较关系（全）相等和（全）不等，结果返回true或者false。

这里需要特别注意的是特殊类型的比较：

```javascript
> null == undefined
true
> null === undefined
false
> NaN == NaN
false
> NaN != NaN
true
> false == 0
true
> true == 1
true
> true == 2
false
> undefined == 0
false
> undefined == false
false
> undefined == NaN
false
```

全等和全不等都增加了类型的比较。

## <a id="conditional"></a> 条件运算符

所谓的条件运算就是通常说的三元运算，其结构如下：

> return_value = condition_expression ? expression_1 : expression_2;

当`condition_expression`为true的时候执行`expression_1`，否则执行`expression_2`，
当然也可以没有返回值。

## <a id="bitwise"></a> 位运算符

位运算指的是把数字转换成32为的二进制数，然后进行与或非、异或、移位等运算。负数的
二进制数的算法是求其补码（对应正数的反码加1）。

- 非：~ 0变1，1变0
- 与：& 1和1才得到1，其他三种情况均是0
- 或：| 只要有一个1就是1，全是0才是0
- 异或：^ 不相同得到1，相同（1和1、0和0）得到0
- 左移：<< 低位补零
- 右移：>> 高位补零（有符号则保留符号位）

更多关于位运算的知识请去百科。

___

# <a id="statement"></a> 语句

## <a id="if"></a> if语句

if语句在任何编程语言中都算是最常用的，其结构为：

    if (condition) statement1 else statement2

`condition`可以是语句，也可以是一个确切的值，不必是Boolean类型（因为其他类型会被
自动转换成Boolean型），如果是true则执行`statement1`，否则执行`statement2`。

还可以串联多个if语句，结构为：

    if (condition1) statement1 else if (condition2) statement2 else statement3

## <a id="switch"></a> switch语句

switch语句和if语句很相像，结构如下：

    switch (expression) {
        case value1:
            statement1;
            break;
        case value2:
            statement2;
            break;
        case value3:
            statement3;
            break;
        default:
            default_statement;
            break;
    }


## <a id="iterative"></a> 循环语句

循环语句通常包括起始条件、终止条件和循环体。

- while

        while (expression) statement

    只要`expression`为true，就会一直执行`statement`。

    ```javascript
    > var i = 1, sum = 0;
    undefined
    > while (i <= 100) { sum += i; ++i; }
    101
    > sum
    5050
    ```

- do...while

        do statement while (expression)

    和while差不多，唯一区别就是先执行后判断，由此看来至少执行一次。

    ```javascript
    > var i = 1, sum = 0;
    undefined
    > do { sum += i; ++i; } while (i <= 100)
    101
    ```

- for

    for循环语句是最常用的。

        for (initialization_condition; expression; post-loop-expression) statement

    初始条件`initialization_condition`在满足`expression`之后执行`statement`并进入
    `post-loop-expression`迭代产生新的条件，然后再去判断是否满足`expression`......

    ```javascript
    > for (var i = sum = 0; i <= 100; i++) { sum += i; }
    5050
    ```

- for...in

        for (property in object) statement

    ```javascript
    // object => property
    > o = {name:'luchanghong',gender:'male',age:25}
    { name: 'luchanghong',
      gender: 'male',
    age: 25 }
    > for (p in o) { console.log(p, o[p]); }
    name luchanghong
    gender male
    age 25
    // array => index
    > for (x in Array(1,2,3)) { console.log(x); }
    0
    1
    2
    ```

## <a id="breakAndContinue"></a> break和continue语句

break和continue都是在执行循环语句过程中跳出当次循环的控制语句。关键看他俩的区别
：break跳出后循环不会再执行，而continue只是跳出一次，在满足条件下还会再次迭代进
入循环。

```javascript
> for (var i = 1; i < 10; i++) { if (0 == i%3) { break; } console.log(i); }
1
2
> for (var i = 1; i < 10; i++) { if (0 == i%3) { continue; } console.log(i); }
1
2
4
5
7
8
```
___

# <a id="function"></a> 函数

## <a id="functionConcept"></a> 函数概述

- 函数结构

    函数是指一组可以调用的代码，和其他编程语言类似，`JavaScript`的函数通常由关键字
    `function`、函数名称、参数、一组代码组成，其结构如下：

        function funcName(arg0, arg1, ... argN) {
            statements
        }

- 函数调用

    函数定义好之后，可以用函数名以及相应的参数调用。例如：

    ```javascript
    > function print_name(name) {
    ... return 'Your name is ' + name;
    ... }
    undefined
    > print_name('luchanghong')
    'Your name is luchanghong'
    ```

- 函数返回值

    如果在函数体内没有`return`语句，那么默认返回值是`undefined`。

## <a id="arguments"></a> arguments对象

在函数体内使用`arguments`对象可以获取输入的参数。按参数的顺序，用`arguments[0]`
就可以得到输入的第一个参数。

可以使用`arguments.length`模拟函数重载，例如：

```javascript
> function fun_arg() {
... if (1 == arguments.length) {
..... return arguments[0];
..... } else if (2 == arguments.length) {
..... return arguments[0]+arguments[1];
..... } else {
..... return 'Wrong arguments number';
..... }
... }
undefined
> fun_arg(10)
10
> fun_arg(10, 20)
30
> fun_arg()
'Wrong arguments number'
> fun_arg(10, 20, 30)
'Wrong arguments number'
```

注意：`JavaScript`函数没有参数默认值的写法，否则报错。

## <a id="functionObject"></a> Function对象

- Function对象

    在`JavaScript`中，函数也是一个对象，而且可以按照下面的格式定义：

        var fun_obj = new Function(arg0, arg1, ... argN, function_body)

    那么这种函数定义如何调用呢？看例子：

    ```javascript
    > var my_fun = new Function('name', 'age', "console.log('My name is ' + name + ', and I am ' + age + ' years old.')")
    undefined
    > my_fun('luchanghong', 25)
    My name is luchanghong, and I am 25 years old.
    ```

    所以，通常定义的函数都可以看做是`Function`对象的实例，那么也就属于引用类型。

- length属性

    `Function`的`length`属性表示函数期望参数的个数，如上例：

    ```javascript
    > my_fun.length
    2
    ```

    由于普通函数是`Function`对象的实例，那么也应该具有此属性：

    ```javascript
    > function fun_1(a, b, c) { return; }
    undefined
    > function fun_2(d, e) { return; }
    undefined
    > console.log(fun_1.length , fun_2.length)
    3 2
    ```

- toString()方法

    `Function`的`toString()`方法返回整个函数的构造源代码，例如：

    ```javascript
    > my_fun.toString()
    'function anonymous(name,age) {\nconsole.log(\'My name is \' + name + \', and I am \' + age + \' years old.\')\n}'
    > fun_1.toString()
    'function fun_1(a, b, c) { return; }'
    ```

虽然使用`Function`也可以定义函数，但是最好不要使用这种方法，因为它比传统的定义
函数要慢得多。

## <a id="closure"></a> closure闭包

闭包是一个很难理解的概念，简单地说就是函数（父函数）内部定义一个函数（子函数）
，当外部调用子函数的时候就产生了闭包。这里就简单的介绍，如果要搞透彻恐怕要长篇
大论了。

由于`JavaScript`中的变量默认是全局变量，而在函数中则是局部变量（但声明的时候必
须加关键字var，否则也是全局变量），这点要特别注意。

所谓全局变量和（函数内）局部变量区别就是：在函数内部可以调用外部的全局变量，而
函数内部的局部变量却不能在函数外部调用，看例子：

```javascript
// 请忽略掉undefined
> var me = 'luchanghong'
undefined
> function fun_me() {
..... var age = 25;
..... gender = 'male';
..... console.log(me)
..... }
undefined
// 函数内部调用外部的全局变量
> fun_me()
luchanghong
undefined
// var声明是局部变量
> console.log(age)
ReferenceError: age is not defined
// 没有var声明式全局变量
> console.log(gender)
male
undefined
```

如果外部需要调用函数内部的变量，那么就要使用闭包了，例如：

```javascript
> function a() {
... var n = 10;
... function b() {
..... return n;
..... }
... return b;
... }
undefined
> var f = a()
undefined
> f()
10
```

这似乎看不出闭包的作用，那继续看：

```javascript
// 接着上面的例子
> f
[Function: b]
> f.toString()
'function b() {\nreturn n;\n}'
```

在执行`f()`的时候，可以看着已经脱离了函数`a()`，但是却可以调用`a()`内部的变量。

闭包另外一个特点就是在使用闭包之后，局部变量并不会释放，继续占用内存，所以使用
的时候要小心，避免内存泄露。更多的信息可以上网查阅。

___

# <a id="objectAndClass"></a> 对象和类

## <a id="objectOriented"></a> 面向对象

- 对象

    `JavaScript`把对象定义为“属性的无序集合，每个属性可以存放一个原始值、对象或
    者函数”。

- 类

    类是对象的原型，定义了对象的接口（interface）和内部关联。

- 实例

    类生成对象的过程叫实例化，生成的对象就是这个类的实例。类和实例可以理解是相同
    的。

    在`JavaScript`的标准`ECMAScript`中并没有正式定义类，而是把对象的定义和描述叫
    做类。

- 对象构成

    在`ECMAScript`中，对象由特性（attribute）构成，特性分为两种：函数这叫做方法
    ，其他成为属性。

## <a id="objectTypes"></a> 对象类型

- 本地对象

    独立于宿主环境（如：浏览器）的对象就是本地对象（native object）：

    - Object
    - Function
    - Array
    - String
    - Boolean
    - Number
    - Date
    - RegExp
    - Error
    - EvalError
    - RangeError
    - ReferenceError
    - SyntaxError
    - TypeError
    - URIError

- 内置对象

    `ECMAScript`中已经被初始化的对象叫做内置对象（built-in object），这些对象已
    经被实例化，可以直接使用。这样的对象有两个：Global和Math。

    ```javascript
    > Math.PI
    3.141592653589793
    > Math.sin(Math.PI/2)
    1
    > Math.sin(Math.PI/6)
    0.49999999999999994
    > Math.abs(-5)
    5
    ```

    Global对象定义一些全局的函数和属性，但实际上Global对象并不存在。前面说到函数
    实际上是对象的一个方法，那么这些内置函数默认就是Global对象的方法了。

- 宿主对象

    和本地对象和内置对象相对的就是宿主对象了，它们都是由宿主环境提供。DOM和BOM对
    象就是宿主对象。

## <a id="objectScope"></a> 对象作用域

- 作用域

    作用域指的是变量的适用范围。在面向对象程序语言中，一个复杂的类一般都有公有、
    私有和保护类的属性和方法。

    - 私有：只有该对象内部才能访问
    - 共有：实例化之后的对象和子类都能访问
    - 保护：主要和私有对象区分，可以被子类继承访问

    但是在`JavaScript`中，类的所有属性和方法都是公用的。为了开发方便，一般用下划
    线区分：

    ```javascript
    person.__name = 'luchanghong';
    ```

- 关键字this

   关键字this确实很关键。this总是指调用它所在方法的对象。

   ```javascript
   > var person = new Object();
   undefined
   > person.name = 'luchanghong';
   'luchanghong'
   > person.printName = function() { return this.name; }
   [Function]
   > person.printName()
   'luchanghong'
   ```

   也许你看了上面的例子觉得this指person对象是顺理成章的事，那么再看：

   ```javascript
   > var book1 = new Object(), book2 = new Object();
   undefined
   > book1.author = 'luchanghong';
   'luchanghong'
   > book2.author = 'moyan';
   'moyan'
   > function printAuthor() { return this.author; }
   undefined
   > book1.sayAuthor = printAuthor
   [Function: printAuthor]
   > book2.sayAuthor = printAuthor
   [Function: printAuthor]
   > book1.sayAuthor()
   'luchanghong'
   > book2.sayAuthor()
   'moyan'
   ```

## <a id="createObject"></a> 创建对象

创建对象很简单，上面例子中`new Object()`就是创建了一个Object类的实例。下面看
几种常见的创建对象的方式：

- 原始方式

    先定义对象，再定义属性和方法。

    ```javascript
    > var Person = new Object();
    undefined
    > Person.name = 'luchanghong';
    'luchanghong'
    > Person.getName = function() { return this.name; }
    [Function]
    > Person.getName();
    'luchanghong'
    ```

    这种方式很直观，属性和方法都一目了然。但若再创建一个对象，只是属性的值或者方
    法内容不一样，就必须把创建对象的过程再写一遍。

    ```javascript
    > var Person2 = new Object();
    undefined
    > Person2.name = 'lu';
    'lu'
    > Person2.getName = function() { return this.name; }
    [Function]
    > Person2.getName();
    'lu'
    ```

    如何把上面两个对象的创建过程提取出来共用？

- 工厂方式

    工厂，顾名思义，有一个模型就能生产出成千上万的成品。看例子：

    ```javascript
    > function createPerson() {
    ... var obj = new Object();
    ... obj.name = 'luchanghong';
    ... obj.getName = function() { return this.name; };
    ... return obj;
    ... }
    undefined
    > var p = createPerson();
    undefined
    > p.getName();
    'luchanghong'
    ```

    看起来有了雏形。如果创建另外一个对象，还得重定义属性岂不很麻烦，那么改进：

    ```javascript
    > function createPerson(name) {
    ... var obj = new Object();
    ... obj.name = name;
    ... obj.getName = function() { return this.name; };
    ... return obj;
    ... }
    undefined
    > var p = createPerson('lu');
    undefined
    > p.getName();
    'lu'
    ```

    对象的属性一般都是原始类型，方法这个函数貌似可以提出来，要不然每个对象就重复
    创建了功能相同的函数了，再改进：

    ```javascript
    > function printName() { return this.name; }
    undefined
    > function createPerson(name) {
    ... var obj = new Object();
    ... obj.name = name;
    ... obj.getName = printName;
    ... return obj;
    ... }
    undefined
    > var p = createPerson('luchanghong');
    undefined
    > p.getName();
    'luchanghong'
    > var p2 = createPerson('lu');
    undefined
    > p2.getName();
    'lu'
    ```

    功能上看起来没什么问题，但这和我们在其他语言中构造对象差别蛮大的，为了迎合
    开发者使用习惯就产生了另一种方式——构造函数。

- 构造函数方式

    构造函数方式和工厂方式差不多，区别在于把函数本身作为构造的对象。看例子：

    ```javascript
    > function Person(name) {
    ... this.name = name;
    ... this.showName = function () { return this.name; };
    ... }
    undefined
    > var p = new Person('lu');
    undefined
    > p
    { name: 'lu', showName: [Function] }
    > p.name
    'lu'
    > p.showName()
    'lu'
    ```

    前面说到函数的时候就一直强调函数也是对象，利用这一点就可以构造出目标对象了。
    有了亲切的`new`，立刻就想到对象了吧。

    同样，在构造函数外部定义函数然后在内部引用，避免重复定义函数。

- 原型方式

    原型方式使用`prototype`来创建对象的。

    ```javascript
    > function Person() { };
    undefined
    > Person.prototype.name = 'lu';
    'lu'
    > Person.prototype.showName = function () { return this.name; };
    [Function]
    > var p = new Person();
    undefined
    > p.showName()
    'lu'
    ```

    用`prototype`去定义对象的属性或者方法，在实例化的时候原型的这些属性和方法都
    被赋予给将要创建的对象。这里的方法（函数）不会被重复创建，只是引用，从而避免
    前面几种方式遇到的问题。

    尽管这种方式看着还不错，但在用的时候会发现这些属性都是固定的，要想改变就必须
    在实例化之后再重新赋值，而且当属性不是简单的原始类型或者函数的时候，多个实例
    化对象会出现混乱，例如：

    ```javascript
    > function Person() { }
    undefined
    > Person.prototype.name = 'lu';
    'lu'
    > Person.prototype.friends = new Array('li', 'hua', 'fang');
    [ 'li', 'hua', 'fang' ]
    > var p1 = new Person();
    undefined
    > var p2 = new Person();
    undefined
    > p1.friends
    [ 'li', 'hua', 'fang' ]
    > p2.friends
    [ 'li', 'hua', 'fang' ]
    > p1.friends.push('123')
    4
    > p2.friends
    [ 'li',
      'hua',
      'fang',
      '123' ]
    > p1.friends
    [ 'li',
      'hua',
      'fang',
      '123' ]
    ```

    这的确是个不小的问题，有什么解决办法呢？

- 混合构造函数/原型方式

    为了解决上面的问题，我们采用这种混合方式。用构造函数的方式定义非函数属性，
    这样就解决了属性值固定的问题；用原型方式定义函数属性，这样解决函数重复定义
    的问题。

    ```javascript
    > function Person(name) {
    ... this.name = name;
    ... this.friends = new Array('li', 'hua');
    ... }
    undefined
    > Person.prototype.makeFriends = function (f) {
    ... this.friends.push(f);
    ... }
    [Function]
    > var p1 = new Person('lu');
    undefined
    > var p2 = new Person('luchanghong');
    undefined
    > p1.friends == p2.friends
    false
    > p1.friends
    [ 'li', 'hua' ]
    > p2.friends
    [ 'li', 'hua' ]
    > p1.makeFriends('911');
    undefined
    > p1.friends
    [ 'li', 'hua', '911' ]
    > p2.friends
    [ 'li', 'hua' ]
    ```

- 动态原型方式

    这种方式的目的是把函数属性拿到构造函数里面来，这样做主要彰显编码风格。例如：

    ```javascript
    > function Person(name) {
    ... this.name = name;
    ... this.friends = new Array('li', 'hua');
    ... if (typeof Person._initialized == 'undefined') {
    ..... Person.prototype.makeFriends = function (f) {
    ....... this.friends.push(f);
    ....... }
    ..... Person._initialized = true;
    ..... }
    ... }
    undefined
    > var p = new Person('lu');
    undefined
    > p.makeFriends('111');
    undefined
    > p.friends
    [ 'li', 'hua', '111' ]
    ```

    用`_initialized`来作为每次实例化对象是否创建函数方法的依据。

以上这么多种方式，到底采用哪种呢？目前采用最多的还是混合构造函数/原型方式，另外，
动态原型方式也是不错的选择。

## <a id="modifyObject"></a> 修改对象

- 修改属性
    
    对象的属性可以直接重新赋值，前面很多例子都有此操作。

- 修改方法

    使用`prototype`可以新增、重写方法。使用前面的`Person`对象举例：

    ```javascript
    > Person.prototype.sayName = function() { return 'I am ' + this.name; }
    [Function]
    // p 是已经实例化过的对象
    > p.sayName()
    'I am lu'
    ```

## <a id="useObject"></a> 使用对象

- 声明和实例化

    对象声明和普通变量一样使用`var`，实例化用关键字`new`。

    ```javascript
    > var obj = new Object();
    undefined
    > var obj2 = new Object;
    undefined
    ```

    当构造函数没有参数的时候，就不用写括号，为了保持一致还是加上括号。

- 引用

    每一次实例化的对象，对象这个变量名存储的是实际对象的引用，而非物理对象自身。

- 销毁

    `ECMAScript`有自动销毁对象释放内存的功能叫做存储单元收集程序。手动销毁将对象
    置为`null`即可。

___

断断续续的算是把`JavaScript`简单的回顾一遍，也涨了不少知识。
