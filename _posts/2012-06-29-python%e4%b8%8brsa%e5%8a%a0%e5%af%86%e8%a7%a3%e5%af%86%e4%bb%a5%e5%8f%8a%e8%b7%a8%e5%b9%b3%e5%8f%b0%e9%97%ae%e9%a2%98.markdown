--- 
wordpress_url: http://luchanghong.com/rosemary/?p=333
wordpress_id: 333
title: !binary |
  cHl0aG9u5LiLUlNB5Yqg5a+G6Kej5a+G5Lul5Y+K6Leo5bmz5Y+w6Zeu6aKY

layout: post
date: 2012-06-29 16:42:25 +08:00
---
项目合作需要，和其他网站通信，消息内容采用RSA加密方式传递。之前没有接触过RSA，于是两个问题出现了：

声明：

环境WIN 7 + python 2.6.6

RSA格式：PEM

一、Python下RSA加密解密怎么做？

现在网上搜索关于RSA的信息，然后看一下Python下是怎么做的。找到两种方法：

1、使用rsa库

安装[shell]pip install rsa[/shell]

可以生成RSA公钥和密钥，也可以load一个.pem文件进来。
<pre>[python]
# -*- coding: utf-8 -*-
__author__ = 'luchanghong'
import rsa

# 先生成一对密钥，然后保存.pem格式文件，当然也可以直接使用
(pubkey, privkey) = rsa.newkeys(1024)

pub = pubkey.save_pkcs1()
pubfile = open('public.pem','w+')
pubfile.write(pub)
pubfile.close()

pri = privkey.save_pkcs1()
prifile = open('private.pem','w+')
prifile.write(pri)
prifile.close()

# load公钥和密钥
message = 'hello'
with open('public.pem') as publickfile:
    p = publickfile.read()
    pubkey = rsa.PublicKey.load_pkcs1(p)

with open('private.pem') as privatefile:
    p = privatefile.read()
    privkey = rsa.PrivateKey.load_pkcs1(p)

# 用公钥加密、再用私钥解密
crypto = rsa.encrypt(message, pubkey)
message = rsa.decrypt(crypto, privkey)
print message

# sign 用私钥签名认真、再用公钥验证签名
signature = rsa.sign(message, privkey, 'SHA-1')
rsa.verify('hello', signature, pubkey)

[/python]</pre>
2、使用M2Crypto

python关于RSA的库还是蛮多的，当然也可以直接用openSSL。M2Crypto安装的时候比较麻烦，虽然官网有exe的安装文件，但是2.6的有bug，建议使用0.19.1版本，最新的0.21.1有问题。
<pre>[python]
# -*- coding: utf-8 -*-
__author__ = 'luchanghong'
from M2Crypto import RSA,BIO

rsa = RSA.gen_key(1024, 3, lambda *agr:None)
pub_bio = BIO.MemoryBuffer()
priv_bio = BIO.MemoryBuffer()

rsa.save_pub_key_bio(pub_bio)
rsa.save_key_bio(priv_bio, None)

pub_key = RSA.load_pub_key_bio(pub_bio)
priv_key = RSA.load_key_bio(priv_bio)

message = 'i am luchanghong'

encrypted = pub_key.public_encrypt(message, RSA.pkcs1_padding)
decrypted = priv_key.private_decrypt(encrypted, RSA.pkcs1_padding)

print decrypted
[/python]</pre>
用法差不多一致。load密钥的方式也有好几种。

二、跨平台密钥不统一

RSA加密验证搞定了，但是和java平台交互的时候出问题，java生成的密钥用Python根本load不了，更别说加密了，反之Java也load不了Python生成的密钥。

上网查找原因，RSA认真流程肯定没有问题，关键是不同平台执行RSA的标准有些偏差。

&nbsp;
