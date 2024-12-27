---
layout: post
title: "国际通用的测试信用卡"
date: 2012-02-19 23:48
comments: true
categories: [web]
---

测试信用卡可以用来系统测试，接收响应，但最后在银行审批的时候会拒绝处理，不会发生真实的交易。

在应用开发阶段，可以用来做卡号合法性检查。

### 可用的测试信用卡:
{% highlight ruby %}
  # MasterCard: 13 or 16 numbers starting with 5
  Master Card  5431111111111111  (16) Characters
  Master Card  5105105105105100  (16) Characters
  Master Card  5555555555554444  (16) Characters
  Master Card  4222222222222 (13) Characters

  # Visa: 13 or 16 numbers starting with 4
  VISA 4111111111111111  (16) Characters
  VISA 4012888888881881  (16) Characters
  VISA 4005550000000019  (16) Characters

  # Amex: 15 numbers starting with 34 or 37
  American Express 34111111111111 (15) Characters
  American Express 378282246310005 (15) Characters
  American Express 371449635398431 (15) Characters
  Amex Corporate 378734493671000 (15) Characters

  #Discover: 16 numbers starting with 6011
  Dinners Club 38520000023237  (14) Characters
  Dinners Club 30569309025904  (14) Characters

  Discover 6011111111111117  (16) Characters
  Discover 6011000990139424  (16) Characters

  JCB  3530111333300000  (16) Characters
  JCB  3566002020360505  (16) Characters
{% endhighlight %}
