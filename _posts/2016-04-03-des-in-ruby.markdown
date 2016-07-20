---
layout: post
title: "DES加密在Ruby中的使用"
date: 2016-04-03 03:00
comments: true
categories: [ruby, aes, interface]
---


![](http://7xifb5.com1.z0.glb.clouddn.com/wustrive-hexo%E5%8A%A0%E8%A7%A3%E5%AF%86.png)
![](http://7xifb5.com1.z0.glb.clouddn.com/RSA%E6%B5%81%E7%A8%8B.png)
图片来自[wustrive's blog](http://wustrive2008.github.io/)

> 数据加密标准（英语：Data Encryption Standard，缩写为 DES）是一种对称密钥加密块密码算法，1976年被美国联邦政府的国家标准局确定为联邦资料处理标准（FIPS），随后在国际上广泛流传开来。它基于使用56位密钥的对称算法。这个算法因为包含一些机密设计元素，相对短的密钥长度以及怀疑内含美国国家安全局（NSA）的后门而在开始时有争议，DES因此受到了强烈的学院派式的审查，并以此推动了现代的块密码及其密码分析的发展。

> DES现在已经不是一种安全的加密方法，主要因为它使用的56位密钥过短。1999年1月，distributed.net与电子前哨基金会合作，在22小时15分钟内即公开破解了一个DES密钥。也有一些分析报告提出了该算法的理论上的弱点，虽然在实际中难以应用。为了提供实用所需的安全性，可以使用DES的派生算法3DES来进行加密，虽然3DES也存在理论上的攻击方法。在2001年，DES作为一个标准已经被高级加密标准（AES）所替换。另外，DES已经不再作为国家标准科技协会（前国家标准局）的一个标准。

> 在某些文献中，作为算法的DES被称为DEA（Data Encryption Algorithm，数据加密算法），以与作为标准的DES区分开来。

> DES是一种典型的块密码—一种将固定长度的平文通过一系列复杂的操作变成同样长度的密文的算法。对DES而言，块长度为64位。同时，DES使用密钥来自定义变换过程，因此算法认为只有持有加密所用的密钥的用户才能解密密文。密钥表面上是64位的，然而只有其中的56位被实际用于算法，其余8位可以被用于奇偶校验，并在算法中被丢弃。因此，DES的有效密钥长度为56位，通常称DES的密钥长度为56位。

DES自身并不是加密的实用手段，而必须以某种工作模式进行实际操作。本文以AES的ECB模式和CBC模式举例实现简单的文本加密解密。

#### ECB模式（电子密码本 Electronic CodeBook）

ECB是最简单的块密码加密模式，加密前根据加密块大小（如AES为128位）分成若干块，之后将每块使用相同的密钥单独加密，解密同理。

![ecb](http://blog.poxiao.me/images/post/63/63d0a979ef5296af4f07b04721614a5ca7c0fc27.png)

![ecb](http://hbimg.b0.upaiyun.com/a988d1d25d3be3ccce073656e7f899a0edaf529b4547-oCiTDq_fw658)

图片来自[happyhippy](http://www.cnblogs.com/happyhippy/archive/2006/12/23/601353.html)

```
优点:
1.简单；
2.有利于并行计算；
3.误差不会被传送；
缺点:
1.不能隐藏明文的模式；
2.可能对明文进行主动攻击；

```

{% highlight ruby %}

# Base64编码的私钥
key = 'baRudSWouiTVfu0jwXfYDg=='

# 加密
text = {message: "这是一段密文"}.to_json
cipher = OpenSSL::Cipher::DES.new("ECB")
cipher.encrypt
cipher.key = Base64.strict_decode64(key)
encrypted = cipher.update(text) + cipher.final
data = Base64.strict_encode64(encrypted)

# 解密
encrypted_string = Base64.decode64(data)
cipher = OpenSSL::Cipher::DES.new("ECB")
cipher.decrypt
cipher.key = Base64.strict_decode64(key)
result = cipher.update(encrypted_string) + cipher.final

{% endhighlight %}

#### CBC模式（密码分组链接：Cipher-Block Chaining)

CBC模式是先将明文切分成若干小段，然后每一小段与初始块或者上一段的密文段进行异或运算后，再与密钥进行加密。

![cbc](http://blog.poxiao.me/images/post/e0/e01c08893db1387653556e5f961253d46c68cf09.png)

![cbc](http://hbimg.b0.upaiyun.com/948694537c1000c19389c815919ce252069295e63d63-zu6Xwa_fw658)

图片来自[happyhippy](http://www.cnblogs.com/happyhippy/archive/2006/12/23/601353.html)

```
优点:
1.不容易主动攻击,安全性好于ECB,适合传输长度长的报文,是SSL、IPSec的标准。
缺点：
1.不利于并行计算；
2.误差传递；
3.需要初始化向量IV

```

{% highlight ruby %}

# Base64编码的私钥
key = 'baRudSWouiTVfu0jwXfYDg=='
iv  = 'abcdefgh3762quck'
# 加密
text = {message: "这是一段密文"}.to_json
cipher = OpenSSL::Cipher::DES.new("CBC")
cipher.encrypt
cipher.key = Base64.strict_decode64(key)
cipher.iv  = iv
encrypted = cipher.update(text) + cipher.final
data = Base64.strict_encode64(encrypted)

# 解密
encrypted_string = Base64.decode64(data)
cipher = OpenSSL::Cipher::DES.new("CBC")
cipher.decrypt
cipher.key = Base64.strict_decode64(key)
cipher.iv  = iv
result = cipher.update(encrypted_string) + cipher.final

{% endhighlight %}