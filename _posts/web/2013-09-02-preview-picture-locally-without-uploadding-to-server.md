---
layout: post
title: 不上传图片直接本地预览
category: web
tags: [javascript,upload]
description: 如果等到上传完毕之后再返回图片URL给用户预览，一旦网络比较慢，用户就得等好久才能预览，而且每次更换图片都会上传到服务器会产生很多垃圾图，考虑这些，不妨直接在本地预览，既快捷又节省带宽和存储空间。
---

## 原理

分为两步：当上传图片的input被触发并选择本地图片之后获取要上传的图片这个对象的URL（对象URL）；把对象URL赋值给事先写好的img标签的src属性即可把图片显示出来。

在这里，我们需要了解Javascript里File对象、Blob对象和window.URL.createObjectURL()方法。

## File对象

> File对象可以用来获取某个文件的信息,还可以用来读取这个文件的内容.通常情况下,File对象是来自用户在一个input元素上选择文件后返回的FileList对象,也可以是来自由拖放操作生成的 DataTransfer对象.

下面来看获取FileList对象：

```javascript
<script type="text/javascript" src="jquery.js"></script>

<input id="upload" type="file">
<img id="preview" src="">

<script type="text/javascript">
$('#upload').change(function(){
    // 获取FileList的第一个元素
    alert(document.getelementbyid('upload').files[0]);
});
</script>
```

## Blob对象

> 一个Blob对象就是一个包含有只读原始数据的类文件对象.Blob对象中的数据并不一定得是JavaScript中的原生形式.File接口基于Blob,继承了Blob的功能,并且扩展支持了用户计算机上的本地文件.

我们想要得到的对象URL实际上就是从Blob这个对象获取的，因为File的接口继承Blob。下面就来把Blob对象转换成URL：

```
<script type="text/javascript">
var f = document.getelementbyid('upload').files[0];
var src = window.URL.createObjectURL(f);
document.getElementById('preview').src = src;
</script>
```

## 兼容性

- 上述方法适用于chrome浏览器
- 如果是IE浏览器可以直接使用input的value来代替src
- 网上查看资料有直接使用File对象的getAsDataURL()方法获取URL的，现在这个方法都已经废除，类似的还有getAsText()和getAsBinary()方法；

## 参考资料 

- [FILE对象][file]
- [Blob对象][blob]
- [window.URL.createObjectURL][url]

[file]: https://developer.mozilla.org/zh-CN/docs/DOM/File
[blob]: https://developer.mozilla.org/zh-CN/docs/DOM/Blob
[url]: https://developer.mozilla.org/zh-CN/docs/DOM/window.URL.createObjectURL
