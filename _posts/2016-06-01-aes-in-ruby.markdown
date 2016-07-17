---
layout: post
title: "AES加密在Ruby中的使用"
date: 2016-06-01 03:00
comments: true
categories: [ruby, aes, interface]
---

![](http://7xifb5.com1.z0.glb.clouddn.com/wustrive-hexo%E5%8A%A0%E8%A7%A3%E5%AF%86.png)
![](http://7xifb5.com1.z0.glb.clouddn.com/wustrive-hexoAES%E6%B5%81%E7%A8%8B.png)
图片来自[wustrive's blog](http://wustrive2008.github.io/)

> 高级加密标准 Advanced Encryption Standard，是美国联邦政府采用的一种区块加密标准。这个标准用来替代原先的DES，已经被多方分析且广为全世界所使用。它是一种分组加密标准，每个加密块大小为128位，允许的密钥长度为128、192和256位。在应用方面，尽管DES在安全上是脆弱的，但由于快速DES芯片的大量生产，使得DES仍能暂时继续使用, 但是迟早要被AES代替。AES在软件及硬件上都能快速地加解密，相对来说较易于实作，且只需要很少的存储器。作为一个新的加密标准，目前正被部署应用到更广大的范围。

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
cipher = OpenSSL::Cipher::AES.new("128-ECB")
cipher.encrypt
cipher.key = Base64.strict_decode64(key)
encrypted = cipher.update(text) + cipher.final
data = Base64.strict_encode64(encrypted)

# 解密
encrypted_string = Base64.decode64(data)
cipher = OpenSSL::Cipher::AES.new("128-ECB")
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
cipher = OpenSSL::Cipher::AES.new("128-CBC")
cipher.encrypt
cipher.key = Base64.strict_decode64(key)
cipher.iv  = iv
encrypted = cipher.update(text) + cipher.final
data = Base64.strict_encode64(encrypted)

# 解密
encrypted_string = Base64.decode64(data)
cipher = OpenSSL::Cipher::AES.new("128-CBC")
cipher.decrypt
cipher.key = Base64.strict_decode64(key)
cipher.iv  = iv
result = cipher.update(encrypted_string) + cipher.final

{% endhighlight %}

