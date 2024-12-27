---
layout: post
title: "安装Ruby Gem 取消文档"
date: 2012-05-23 23:48
comments: true
categories: [Ruby, Gem]
---

单个Gem不安装文档

{% highlight ruby %}
gem install honeybadger --no-rdoc --no-ri
{% endhighlight %}

如果要全局不安装文档，则在rubygems配置文件`~/.gemrc`

{% highlight ruby %}
gem: --no-document
{% endhighlight %}