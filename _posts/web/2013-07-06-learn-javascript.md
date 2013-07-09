---
layout: post
category: web
title: Learn JavaScript
tags: [javascript]
description: JavaScript学习概述
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

    - 函数概述
    - arguments对象
    - Function对象
    - Closure闭包

- [对象和类](#object&class)

    - 面向对象
    - 对象类型
    - 对象作用域this
    - 创建对象
    - 修改对象
    - 使用对象

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

