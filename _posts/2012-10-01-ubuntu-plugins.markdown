---
layout: post
title: "Ubuntu的几个插件"
date: 2012-10-01 14:55
comments: true
categories: [ubuntu]
description: "Ubuntu的几个插件: 防止显示器休眠, 系统负载"
---
##Caffeine

>防止显示器休眠

{% highlight ruby %}
sudo add-apt-repository ppa:caffeine-developers/ppa
sudo apt-get update
sudo apt-get install caffeine
{% endhighlight %}

##System Load / Performance

>系统负载

{% highlight ruby %}
sudo apt-get install indicator-multiload
{% endhighlight %}

