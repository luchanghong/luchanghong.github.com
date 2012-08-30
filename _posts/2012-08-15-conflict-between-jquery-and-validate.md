--- 
wordpress_url: http://luchanghong.com/rosemary/?p=497
wordpress_id: 497
title: !binary |
  anF1ZXJ5IHZhbGlkYXRlIOS4jiBvbnN1Ym1pdOeahOWGsueqgeWwj+iusA==

layout: post
date: 2012-08-15 23:06:17 +08:00
---
最近写了点前端代码，验证form表单的时候用到了jquery validate。这个东西很好用，具体的使用方法就不必啰嗦了。

这个东西主要是用来验证表单数据的格式，有时候虽然格式正确了但是还要保证某些字段的唯一性，就要另外判断了，我用的方法是在form加一个onsubmit事件。

测试的时候发现，只要满足validate验证的格式，onsubmit的判断会被忽略掉。事实上，jquery validate也是在submit的时候开始判断的，并且在onsubmit事件之后，所以onsubmit返回false，只要满足validate的条件仍然会返回true，表单仍被提交。

明白这个原因之后，解决方法也就很简单针对了：

在构造validate规则的时候加上submitHandler，如：
<pre>[javascript]

        $("#finaceAccount").validate({
            rules:{name :{required :true,minlength:2},
            },
            messages: {
                name: {required: "请输入姓名！",minlength: "请填写全名！"},
            },
            submitHandler: function (form){
                return checkInfo(form);
            }
        });

        function checkInfo(f){
            ……
            return false
        }
[/javascript]</pre>
这样就OK了，也就可以把onsubmit事件删除了。

<span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">补充：checkInfo()函数里面有ajax验证，这样写也不行，我直接在submitHandler返回true但表单并未提交，研究半天也没找到什么解决办法，只好换其他验证方法了。至此这个冲突还没有解决……我也不是专业前端开发人员，也没太多时间去网上找答案，就暂时避开此方法了，以后有空再看吧，坑爹啊！</span></span>
